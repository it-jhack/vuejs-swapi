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
          :items="response.data.results.name"
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
            <v-expansion-panel-header> charNames </v-expansion-panel-header>
            <v-expansion-panel-content>
              {{ charNames }}
            </v-expansion-panel-content>
          </v-expansion-panel>

          <v-expansion-panel>
            <v-expansion-panel-header> charMovies </v-expansion-panel-header>
            <v-expansion-panel-content>
              {{ charMovies }}
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
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
    charNames: [],
    charMovies: [],
    value: null,
  }),
  methods: {
    async getAllChars() {
      this.response = await axios.get("https://swapi.dev/api/people/");

      this.response.data.results.forEach((result) => {
        this.charNames.push(result.name);
        // let index = result.
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
