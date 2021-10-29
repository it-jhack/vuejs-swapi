<template>
  <v-footer dark padless>
    <v-card flat tile class="secondary white--text text-center">
      <v-card-text>
        {{ new Date().getFullYear() }} — by Thiago Amaral &nbsp;&nbsp;

        <v-btn
          v-for="(icon, i) in icons"
          :key="i"
          class="mx-4 white--text"
          :href="icon.link"
          target="_blank"
          icon
        >
          <v-icon size="24px">{{ icon.text }}</v-icon>
        </v-btn>
      </v-card-text>
    </v-card>
  </v-footer>
</template>

<style scoped>
.v-card {
  width: 100%;
}
</style>

<script>
export default {
  data: () => ({
    icons: [
      {
        text: "mdi-github",
        link: "https://github.com/it-jhack",
      },
      {
        text: "mdi-linkedin",
        link: "https://www.linkedin.com/in/thiago-de-paula-amaral/",
      },
    ],
  }),
};
</script>
