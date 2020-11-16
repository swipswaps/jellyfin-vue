<template>
  <v-form ref="form" v-model="validInputs">
    <v-text-field
      v-model="admin.Name"
      outlined
      :label="$t('username')"
      type="username"
      required
    ></v-text-field>
    <v-text-field
      v-model="admin.Password"
      outlined
      :label="$t('password')"
      :rules="[(v) => !!v || $t('passwordRequired')]"
      required
      :append-icon="showPassword ? 'mdi-eye-off' : 'mdi-eye'"
      :type="showPassword ? 'text' : 'password'"
      @click:append="() => (showPassword = !showPassword)"
    ></v-text-field>
    <v-text-field
      v-model="passwordCheck"
      outlined
      :label="$t('confirmPassword')"
      required
      type="password"
      :rules="[(v) => (!!v && v === admin.Password) || $t('bothPasswordsSame')]"
    ></v-text-field>
    <v-btn text @click="$emit('previous-step', { step: 2 })">
      {{ $t('previous') }}
    </v-btn>
    <v-btn color="primary" :disabled="!validInputs" @click="createAdminAccount">
      {{ $t('next') }}
    </v-btn>
  </v-form>
</template>

<script lang="ts">
import Vue from 'vue';
import { mapActions } from 'vuex';

export default Vue.extend({
  data() {
    return {
      admin: {
        Name: '',
        Password: ''
      },
      passwordCheck: '',
      showPassword: false,
      validInputs: false,
      loading: false
    };
  },
  methods: {
    ...mapActions('deviceProfile', ['setDeviceProfile']),
    ...mapActions('snackbar', ['pushSnackbarMessage']),
    async createAdminAccount() {
      try {
        this.setDeviceProfile();
        const token = `MediaBrowser Client="${this.$auth.ctx.app.store.state.deviceProfile.clientName}", Device="${this.$auth.ctx.app.store.state.deviceProfile.deviceName}", DeviceId="${this.$auth.ctx.app.store.state.deviceProfile.deviceId}", Version="${this.$auth.ctx.app.store.state.deviceProfile.clientVersion}"`;
        this.$auth.ctx.app.$axios.setHeader('X-Emby-Authorization', token);

        await this.$api.startup.updateStartupUser({
          startupUserDto: this.admin
        });

        this.pushSnackbarMessage({
          message: this.$t('userSuccessfullySet'),
          color: 'success'
        });

        this.$emit('step-complete', { step: 2 });
      } catch (e) {
        console.error(e);
      }
    }
  }
});
</script>
