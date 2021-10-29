<template>
  <v-container>
    <v-row>
      <v-col cols="10" class="center">
        <br />
        <v-text center
          >Search or select a Star Wars character, and see which movies it's
          been in!</v-text
        >
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="10" class="center">
        <v-autocomplete
          @change="getCharMovies"
          v-model="value"
          :items="characters"
          item-text=".name"
          filled
          dark
          label="Search or select character"
          hide-no-data
        ></v-autocomplete>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="10" class="center">
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
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",

  data: () => ({
    response: [],
    characters: [],
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
      // Chars come in group of 10 per page
      this.response = await axios.get("https://swapi.dev/api/people/");
      const count = this.response.data.count;
      const pages = Math.ceil(count / 10);

      // I opted not filtering each char data that is relevant for now because:
      // -in case we want to explore more data for each char in the future
      // -this request is only made once (when loading the page)
      this.response.data.results.forEach((result) => {
        this.characters.push(result);
      });

      // Chars from results on first page already gotten previously, so now page 2 until end
      for (let i = 2; i <= pages; i++) {
        this.response = await axios.get(
          "https://swapi.dev/api/people/?page=" + i
        );
        this.response.data.results.forEach((result) => {
          this.characters.push(result);
        });
      }
    },

    getCharMovies() {
      this.moviesData = []; //resetting array, so it does not pile up with previous selections

      this.characters.forEach((char) => {
        if (char.name == this.value) {
          this.charMoviesApiUrl = char.films;
        }
      });

      // Request each movie url listed under selected char
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

.center {
  margin-left: auto;
  margin-right: auto;
}
</style>
