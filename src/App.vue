<template>
  <v-app>
    <v-app-bar dense app>
      <v-toolbar-title class="headline">
        <span>Kulmi</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn small v-if="getCurrentRoute() != '/'">
          <router-link to="/" tag="div"><v-icon>mdi-home</v-icon></router-link>
      </v-btn>
    </v-app-bar>

    <v-content>
        <router-view no-gutters></router-view>
    </v-content>
  </v-app>
</template>

<script>
import KulmiGrid from './components/KulmiGrid';
import LandingScreen from './components/LandingScreen';
import VueRouter from 'vue-router';

const routes = [
    { path: '/', name: 'home', component: LandingScreen },
    { path: '/jass', name: 'jass', component: KulmiGrid, props: true}
]

const router = new VueRouter({
    routes
})

export default {
  name: 'App',
  router: router,
  data: () => ({
  }),
  methods: {
      getCurrentRoute() {
          return this.$route.path;
      }
  }
};
</script>