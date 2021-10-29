<template>
  <v-app>
    <v-app-bar app color="dark-grey" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="@/assets/sw-logo.png"
          transition="scale-transition"
          width="40"
        />
        Star Wars API Demonstration
      </div>

      <v-spacer></v-spacer>

      <v-btn href="https://swapi.dev/" target="_blank" text>
        <span class="mr-2">Swapi Docs</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main class="dark-bg">
      <LandingPage />
    </v-main>
    <MyFooter />
  </v-app>
</template>

<script>
import LandingPage from "./components/LandingPage";
import MyFooter from "./components/Footer";

export default {
  name: "App",

  components: {
    LandingPage,
    MyFooter,
  },

  data: () => ({
    //
  }),
};
</script>

<style scoped>
.dark-bg {
  background-color: rgb(37, 37, 37);
}
</style>
