<template>
  <v-container>
    <v-row align="center">
      <v-col cols="12">
        <br />
        <v-text center
          >Select a Star Wars Character, and see which movies it's been
          in!</v-text
        >
      </v-col>
    </v-row>
    <v-row align="center">
      <v-col cols="12">
        <v-autocomplete
          @change="getCharMovies"
          v-model="value"
          :items="charNames"
          filled
          dark
          label="Search or select character"
        ></v-autocomplete>
        <!-- #! BEGIN DELETE -->
        <!-- <v-text>{{ characters }}</v-text
        ><br /> -->
        <v-text>{{ charNames }}</v-text
        ><br />
        <v-text>{{ charMovies }}</v-text
        ><br />
        <!-- #! END DELETE -->
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
    results: [],
    charNames: [],
    charMovies: [],
    value: null,
  }),
  methods: {
    async getAllChars() {
      this.response = await axios
        .get("https://swapi.dev/api/people/")
        .then((response) => (this.results = response.data.results));

      for (let result in this.results) {
        this.charNames.push(result.name);
      }
    },
    // getCharMovies() {
    //   alert("getCharMovies");
    // },
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
