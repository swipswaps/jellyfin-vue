<template>
  <div>
    <v-select
      v-model="UICulture"
      outlined
      :label="$t('preferedLanguage')"
      required
      item-text="Name"
      item-value="Value"
      :items="culturesList"
    />
    <v-btn color="primary" @click="setLanguage"> {{ $t('next') }} </v-btn>
  </div>
</template>

<script>
import Vue from 'vue';
import { mapActions } from 'vuex';

export default Vue.extend({
  auth: false,
  data() {
    return {
      UICulture: 'en-GB',
      culturesList: [],
      initialConfig: {}
    };
  },
  async created() {
    try {
      // This baseurl is only set since #122 is not merged.
      // Once #122 is merged, the baseURL will be set there.
      this.$axios.setBaseURL('http://localhost:8096');

      this.initialConfig = (
        await this.$api.startup.getStartupConfiguration()
      ).data;

      this.UICulture = this.initialConfig.UICulture;

      this.culturesList = (
        await this.$api.localization.getLocalizationOptions()
      ).data;
    } catch (e) {
      console.error(e);
    }
  },
  methods: {
    ...mapActions('snackbar', ['pushSnackbarMessage']),
    setLanguage() {
      try {
        this.$api.startup.updateInitialConfiguration({
          startupConfigurationDto: {
            ...this.initialConfig,
            UICulture: this.UICulture
          }
        });

        this.pushSnackbarMessage({
          message: this.$t('languageSuccessfullySet'),
          color: 'success'
        });

        this.$emit('step-complete', { step: 1 });
      } catch (e) {
        console.error(e);
      }
    }
  }
});
</script>
