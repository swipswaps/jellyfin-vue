<template>
  <v-container fill-height>
    <v-row align="center" justify="center">
      <v-col md="4">
        <h1 class="text-h4 mb-6 text-center">{{ heading }}</h1>
        <h1 class="text-h6 mb-6 text-center">{{ subHeading }}</h1>
        <v-stepper v-model="wizardStage" class="tran">
          <v-stepper-header>
            <v-stepper-step :complete="wizardStage > 1" step="1">
              {{ $t('languageLocale') }}
            </v-stepper-step>

            <v-divider></v-divider>

            <v-stepper-step :complete="wizardStage > 2" step="2">
              {{ $t('administratorAccount') }}
            </v-stepper-step>

            <v-divider></v-divider>

            <v-stepper-step :complete="wizardStage > 3" step="3">
              {{ $t('preferredMetadataLanguage') }}
            </v-stepper-step>
            <v-divider></v-divider>

            <v-stepper-step :complete="wizardStage > 4" step="4">
              {{ $t('setupLibraries') }}
            </v-stepper-step>
            <v-divider></v-divider>

            <v-stepper-step :complete="wizardStage > 5" step="5">
              {{ $t('remoteAccess') }}
            </v-stepper-step>
          </v-stepper-header>

          <v-stepper-items>
            <v-stepper-content step="1">
              <wizard-language @step-complete="changeStep" />
            </v-stepper-content>

            <v-stepper-content step="2">
              <wizard-admin-account
                @step-complete="changeStep"
                @previous-step="previousStep"
              />
            </v-stepper-content>

            <v-stepper-content step="3">
              <wizard-metadata
                @step-complete="changeStep"
                @previous-step="previousStep"
              />
            </v-stepper-content>

            <v-stepper-content step="4">
              <wizard-add-libraries
                @step-complete="changeStep"
                @previous-step="previousStep"
              />
            </v-stepper-content>

            <v-stepper-content step="5">
              <wizard-remote-access
                @step-complete="changeStep"
                @previous-step="previousStep"
              />
            </v-stepper-content>
          </v-stepper-items>
        </v-stepper>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import Vue from 'vue';
import { mapActions } from 'vuex';

export default Vue.extend({
  layout: 'fullpage',
  auth: false,
  data() {
    return {
      wizardStage: 1
    };
  },
  head() {
    return {
      title: this.$store.state.page.title
    };
  },
  computed: {
    heading() {
      switch (this.wizardStage) {
        case 1:
          return this.$t('languageLocale');
        case 2:
          return this.$t('administratorAccount');
        case 3:
          return this.$t('preferredMetadataLanguage');
        case 4:
          return this.$t('setupLibraries');
        case 5:
          return this.$t('remoteAccess');
      }
      return '';
    },
    subHeading() {
      switch (this.wizardStage) {
        case 2:
          return this.$t('adminAccountDescription');
        case 3:
          return this.$t('configureLanguagesPerLibrary');
      }
      return '';
    }
  },
  created() {
    this.setPageTitle({ title: this.$t('login') });
  },
  methods: {
    ...mapActions('page', ['setPageTitle']),
    async completeWizard() {
      try {
        await this.$api.startup.completeWizard();
        // Redirect to setup complete page
      } catch (e) {
        console.error(e);
      }
    },
    changeStep({ step }: { step: number }) {
      if (step === 5) this.completeWizard();
      else this.wizardStage += 1;
    },
    previousStep() {
      this.wizardStage -= 1;
    }
  }
});
</script>

<style scoped>
.tran {
  background-color: transparent !important;
}
</style>
