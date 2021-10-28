<template>
  <v-container>
    <v-row align="center">
      <v-col cols="12">
        <br />
        <v-text center
          >Search or select a Star Wars character, and see which movies it's
          been in!</v-text
        >
      </v-col>
    </v-row>

    <v-row align="center">
      <v-col cols="12">
        <v-autocomplete
          @change="getCharMovies"
          v-model="value"
          :items="response.data.results"
          item-text=".name"
          filled
          dark
          label="Search or select character"
        ></v-autocomplete>
        <!-- #! BEGIN DELETE -->
        <v-expansion-panels dark multiple>
          <v-expansion-panel>
            <v-expansion-panel-header> response </v-expansion-panel-header>
            <v-expansion-panel-content>
              {{ response }}
            </v-expansion-panel-content>
          </v-expansion-panel>

          <v-expansion-panel>
            <v-expansion-panel-header>
              charMoviesApiUrl
            </v-expansion-panel-header>
            <v-expansion-panel-content>
              {{ charMoviesApiUrl }}
            </v-expansion-panel-content>
          </v-expansion-panel>

          <v-expansion-panel>
            <v-expansion-panel-header> moviesData </v-expansion-panel-header>
            <v-expansion-panel-content>
              {{ moviesData }}
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
        <!-- #! END DELETE -->
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="8">
        <h1 v-if="value != null">Movies that {{ value }} participated:</h1>
        Lorem ipsum dolor sit amet consectetur, adipisicing elit. Accusantium
        accusamus blanditiis, beatae dolore consectetur, impedit saepe porro ex
        voluptatibus provident fugiat velit. Necessitatibus autem quod neque
        quae porro, impedit unde maxime ex eius expedita? Id, illum repellendus?
        Nam excepturi molestiae asperiores consequuntur, consequatur incidunt
        neque debitis suscipit quasi praesentium architecto?
      </v-col>
      <v-col cols="4">
        Lorem, ipsum dolor sit amet consectetur adipisicing elit. Libero,
        pariatur.
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",

  data: () => ({
    response: [],
    charMoviesApiUrl: null,
    moviesData: [],
    value: null,
  }),
  methods: {
    async getAllChars() {
      this.response = await axios.get("https://swapi.dev/api/people/");
    },

    getCharMovies() {
      this.response.data.results.forEach((result) => {
        if (result.name == this.value) {
          this.charMoviesApiUrl = result.films;
        }
      });
      this.charMoviesApiUrl.forEach((movieApiUrl) => {
        // this.moviesData.push(axios.get(movieApiUrl));
        // this.moviesData.push(async await axios.get("https://swapi.dev/api/people/"));
        axios
          .get(movieApiUrl)
          .then((response) => this.moviesData.push(response));
      });
    },
  },
  mounted() {
    this.getAllChars();
  },
};
</script>

<style scoped>
v-text {
  color: rgb(255, 255, 255);
  text-align: center;
  font-size: 20px;
}
</style>
