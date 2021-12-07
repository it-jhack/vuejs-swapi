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
      <br /><br />
    </v-row>

    <v-row>
      <v-col cols="10" class="center">
        <v-autocomplete
          @change="getCharMovies()"
          v-model="selectedCharName"
          :items="characters"
          item-text=".name"
          filled
          dark
          label="Search or select character"
          hide-no-data
        >
        </v-autocomplete>
      </v-col>
    </v-row>

    <v-row justify="center">
      <v-col cols="7" order="1">
        <!-- Lorem, ipsum dolor sit amet consectetur adipisicing elit. Sit tempora
        accusamus neque laboriosam accusantium eaque recusandae asperiores
        mollitia placeat culpa, rerum perspiciatis! Autem doloribus voluptates
        dignissimos atque blanditiis? Magnam quod rem aperiam corrupti atque
        blanditiis nihil mollitia. Libero minus nihil nobis, quidem perspiciatis
        sed error tenetur et obcaecati accusamus. Expedita!

        <br /><br />

        Lorem, ipsum dolor sit amet consectetur adipisicing elit. Sit tempora
        accusamus neque laboriosam accusantium eaque recusandae asperiores
        mollitia placeat culpa, rerum perspiciatis! Autem doloribus voluptates
        dignissimos atque blanditiis? Magnam quod rem aperiam corrupti atque
        blanditiis nihil mollitia. Libero minus nihil nobis, quidem perspiciatis
        sed error tenetur et obcaecati accusamus. Expedita! -->
      </v-col>
      <v-col cols="3" align-self="start" order="0" v-if="!isMobile()">
        <v-img
          :src="charImgUrl"
          lazy-src="src/assets/unknown-char.png"
          v-if="charImgUrl"
        >
          <template v-slot:placeholder>
            <v-row class="fill-height ma-0" align="center" justify="center">
              <v-progress-circular indeterminate color="grey lighten-5">
              </v-progress-circular>
            </v-row>
          </template>
        </v-img>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="10" class="center" v-if="!isMobile()">
        <h2 v-if="selectedCharName != null">
          Movies that {{ selectedCharName }} participated in:
        </h2>
        <v-data-table
          v-if="selectedCharName != null"
          hide-default-footer
          dark
          dense
          :loading="loadingTable"
          loading-text="Loading it is. Patient you must be!"
          :sort-by.sync="sortBy"
          :sort-desc.sync="sortDesc"
          must-sort
          :headers="tbHeaders"
          :items="moviesData"
          item-key="name"
          class="elevation-1"
        >
          <template v-slot:[`item.release_date`]="{ item }">
            {{ displayDate(item.release_date) }}
          </template>
        </v-data-table>
      </v-col>
    </v-row>
    <br /><br />
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  data: () => ({
    response: [],
    characters: [],
    charMoviesApiUrl: null,
    moviesData: [],
    loadingTable: true,
    selectedCharName: null,
    sortBy: "release_date",
    sortDesc: false,

    // Api keys:
    VUE_APP_AZURE_SUBSCRIPTION_KEY: process.env.VUE_APP_AZURE_SUBSCRIPTION_KEY,
    // How to get one: https://docs.microsoft.com/en-us/bing/search-apis/bing-custom-search/how-to/quick-start
    VUE_APP_CUSTOM_CONFIG: process.env.VUE_APP_CUSTOM_CONFIG,
    // How to get one: https://docs.microsoft.com/en-us/azure/cognitive-services/bing-custom-search/quick-start

    // Remember to set your api keys in a '.env.local' file inside the project;
    // See: https://cli.vuejs.org/guide/mode-and-env.html#modes and https://youtu.be/XIptuxLGDyk
    // You should never hardcode or upload api keys to repositories (even private ones), as that's a critical
    // security risk.

    // To safely deploy them on simple live applications:
    // https://www.freecodecamp.org/news/private-api-keys/

    count: 1,
    mkt: "en-US",
    imgSearchResponse: null,
    charImgUrl: null,

    tbHeaders: [
      {
        text: "Film Title",
        align: "start",
        value: "title",
      },
      { text: "Director(s)", value: "director" },
      { text: "Producer(s)", value: "producer" },
      { text: "Release Date (year-month-day)", value: "release_date" },
    ],
  }),

  methods: {
    displayDate(dateString) {
      let p = dateString.split(/\D/g);
      return [p[2], p[1], p[0]].join("/");
    },

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

    async getCharMovies() {
      this.bingWebSearch("freddy mercury success meme");

      this.loadingTable = true;
      this.moviesData = []; //resetting array, so it does not pile up with previous selections

      this.characters.forEach((char) => {
        if (char.name == this.selectedCharName) {
          this.charMoviesApiUrl = char.films;
        }
      });

      // Request each movie url listed under selected char
      await this.charMoviesApiUrl.forEach((movieApiUrl) => {
        axios.get(movieApiUrl).then((response) =>
          this.moviesData.push({
            title: response.data.title,
            director: response.data.director,
            producer: response.data.producer,
            release_date: response.data.release_date,
          })
        );
      });
      this.loadingTable = "false";
    },

    bingWebSearch() {
      const https = require("https");

      this.imgSearchResponse = https.get(
        {
          hostname: "api.bing.microsoft.com",
          path:
            "/v7.0/custom/images/search?q=" +
            encodeURIComponent(this.selectedCharName) +
            "&count=" +
            this.count +
            "&customConfig=" +
            this.VUE_APP_CUSTOM_CONFIG +
            "&mkt=" +
            this.mkt,
          headers: {
            "Ocp-Apim-Subscription-Key": this.VUE_APP_AZURE_SUBSCRIPTION_KEY,
          },
        },
        (res) => {
          let body = "";
          res.on("data", (part) => (body += part));
          res.on("end", () => {
            for (var header in res.headers) {
              if (
                header.startsWith("bingapis-") ||
                header.startsWith("x-msedge-")
              ) {
                console.log(header + ": " + res.headers[header]);
              }
            }
            let responseObj = JSON.parse(body);
            this.charImgUrl = responseObj.value[0].thumbnailUrl;
          });
          res.on("error", (e) => {
            console.log("Error: " + e.message);
            throw e;
          });
        }
      );
    },

    isMobile() {
      if (
        /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(
          navigator.userAgent
        )
      ) {
        return true;
      } else {
        return false;
      }
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
