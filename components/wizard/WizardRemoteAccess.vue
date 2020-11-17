<template>
  <div>
    <v-checkbox v-model="allowRemoteAccess" :label="$t('allowRemoteAccess')" />
    <v-checkbox v-model="enableUPNP" :label="$t('enableUPNP')" />
    <v-btn text @click="$emit('previous-step')">
      {{ $t('previous') }}
    </v-btn>
    <v-btn color="primary" @click="setRemoteAccess"> {{ $t('finish') }} </v-btn>
  </div>
</template>

<script>
import Vue from 'vue';

export default Vue.extend({
  data() {
    return {
      allowRemoteAccess: false,
      enableUPNP: false
    };
  },
  methods: {
    async setRemoteAccess() {
      try {
        await this.$api.startup.setRemoteAccess({
          startupRemoteAccessDto: {
            EnableRemoteAccess: this.allowRemoteAccess,
            EnableAutomaticPortMapping: this.enableUPNP
          }
        });

        this.pushSnackbarMessage({
          message: this.$t('remoteAccessSettingsSettingsSet'),
          color: 'success'
        });

        this.$emit('step-complete', { step: 5 });
      } catch (e) {
        console.error(e);
      }
    }
  }
});
</script>
