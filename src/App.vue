<template>
  <v-app>
    <v-app-bar dense app>
      <v-toolbar-title class="headline">
        <span>Kulmi</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        v-if="false"
        text
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
      >
        <span class="mr-2">Latest Release</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <LandingScreen />
    </v-content>
  </v-app>
</template>

<script>
//import KulmiGrid from './components/KulmiGrid';
import LandingScreen from './components/LandingScreen';

export default {
  name: 'App',
  components: {
    //KulmiGrid,
    LandingScreen,
  },
  data: () => ({
    //
  }),
};
</script>