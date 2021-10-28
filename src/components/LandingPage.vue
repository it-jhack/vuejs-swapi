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
        <h2 v-if="value != null">Movies that {{ value }} participated:</h2>
        <v-data-table
          v-if="value != null"
          hide-default-footer
          dark
          dense
          :headers="headers"
          :items="moviesData"
          item-key="name"
          class="elevation-1"
        ></v-data-table>
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
    headers: [
      {
        text: "Film Title",
        align: "start",
        sortable: false,
        value: "title",
      },
      { text: "Director(s)", value: "director" },
      { text: "Producer(s)", value: "producer" },
      { text: "Release Date (day/month/year)", value: "release_date" },
    ],
  }),
  methods: {
    async getAllChars() {
      this.response = await axios.get("https://swapi.dev/api/people/");
    },

    getCharMovies() {
      this.moviesData = []; //resetting array, so it does not pile up with previous selections

      this.response.data.results.forEach((result) => {
        if (result.name == this.value) {
          this.charMoviesApiUrl = result.films;
        }
      });
      this.charMoviesApiUrl.forEach((movieApiUrl) => {
        axios.get(movieApiUrl).then((response) =>
          this.moviesData.push({
            title: response.data.title,
            director: response.data.director,
            producer: response.data.producer,
            release_date: this.convertDate(response.data.release_date),
          })
        );
      });
    },

    convertDate(dateString) {
      let p = dateString.split(/\D/g);
      return [p[2], p[1], p[0]].join("/");
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

h2 {
  color: rgb(255, 255, 255);
}
</style>
