<template>
  <v-container fluid>
    <v-row>
      <v-col cols="9" md="3" class="d-flex flex-row pa-0">
        <v-avatar tile size="72" color="primary">
          <v-icon dark>mdi-album</v-icon>
        </v-avatar>
        <div class="d-flex flex-column justify-center ml-4">
          <span class="font-weight-medium">Song Title Goes Here</span>
          <span class="text-caption">Artist Goes Here</span>
        </div>
      </v-col>
      <v-col cols="6" class="pa-0 d-none d-md-inline">
        <div class="d-flex flex-column justify-center">
          <div class="d-flex align-center justify-center">
            <v-btn icon class="mx-1">
              <v-icon>mdi-shuffle</v-icon>
            </v-btn>
            <v-btn icon class="mx-1">
              <v-icon>mdi-skip-previous</v-icon>
            </v-btn>
            <v-btn icon large class="mx-1">
              <v-icon large>mdi-play-circle-outline</v-icon>
            </v-btn>
            <v-btn icon class="mx-1">
              <v-icon>mdi-skip-next</v-icon>
            </v-btn>
            <v-btn icon class="mx-1">
              <v-icon>mdi-repeat-off</v-icon>
            </v-btn>
          </div>
          <v-slider hide-details min="0" max="100" value="50">
            <template #prepend><span class="mt-1">0:00</span></template>
            <template #append><span class="mt-1">4:20</span></template>
          </v-slider>
        </div>
      </v-col>
      <v-col
        offset="1"
        cols="2"
        class="d-none d-md-flex align-center justify-end"
      >
        <v-tooltip top>
          <template #activator="{ on, attrs }">
            <v-btn icon class="mr-2" v-bind="attrs" v-on="on">
              <v-icon>mdi-playlist-play</v-icon>
            </v-btn>
          </template>
          <span>{{ $t('queue') }}</span>
        </v-tooltip>
        <v-slider
          hide-details
          min="0"
          max="100"
          value="100"
          prepend-icon="mdi-volume-high"
        >
        </v-slider>
        <v-tooltip top>
          <template #activator="{ on, attrs }">
            <v-btn icon class="ml-2" v-bind="attrs" v-on="on">
              <v-icon>mdi-fullscreen</v-icon>
            </v-btn>
          </template>
          <span>{{ $t('fullScreen') }}</span>
        </v-tooltip>
      </v-col>
      <v-col cols="3" class="d-flex d-md-none px-0 align-center justify-end">
        <v-btn icon>
          <v-icon>mdi-heart</v-icon>
        </v-btn>
        <v-btn icon>
          <v-icon>mdi-play</v-icon>
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>
