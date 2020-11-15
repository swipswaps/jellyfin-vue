<template>
  <div>
    <v-select
      v-model="metadataLanguage"
      outlined
      :label="$t('metadataLanguage')"
      required
      item-text="DisplayName"
      item-value="TwoLetterISOLanguageName"
      :items="cultureOptions"
    />
    <v-select
      v-model="metadataCountry"
      outlined
      :label="$t('metadataCountry')"
      required
      item-text="DisplayName"
      item-value="TwoLetterISORegionName"
      :items="countryOptions"
    />
    <v-btn text @click="$emit('previous-step', { step: 2 })">
      {{ $t('previous') }}
    </v-btn>
    <v-btn primary @click="setMetadata">{{ $t('next') }}</v-btn>
  </div>
</template>

<script>
import Vue from 'vue';
import { mapActions } from 'vuex';

export default Vue.extend({
  data() {
    return {
      metadataLanguage: '',
      metadataCountry: '',
      initialConfig: {},
      cultureOptions: [],
      countryOptions: []
    };
  },
  async created() {
    this.initialConfig = (
      await this.$api.startup.getStartupConfiguration()
    ).data;

    this.metadataLanguage = this.initialConfig.MetadataCountryCode;
    this.metadataCountry = this.initialConfig.PreferredMetadataLanguage;

    this.cultureOptions = (await this.$api.localization.getCultures()).data;
    this.countryOptions = (await this.$api.localization.getCountries()).data;
  },
  methods: {
    ...mapActions('snackbar', ['pushSnackbarMessage']),
    async setMetadata() {
      try {
        await this.$api.startup.updateInitialConfiguration({
          startupConfigurationDto: {
            ...this.initialConfig,
            MetadataCountryCode: this.metadataLanguage,
            PreferredMetadataLanguage: this.metadataCountry
          }
        });

        this.pushSnackbarMessage({
          message: this.$t('metadataLanguagesSet'),
          color: 'success'
        });

        this.$emit('step-complete', { step: 3 });
      } catch (e) {
        console.error(e);
      }
    }
  }
});
</script>
