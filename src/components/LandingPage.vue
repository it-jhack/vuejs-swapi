<template>
  <v-container>
    <v-row align="center">
      <v-col cols="12">
        <v-text center
          >Select a Star Wars Character, and see which movies it's been
          in!</v-text
        >
      </v-col>
    </v-row>
    <v-row align="center">
      <v-col cols="12">
        <v-autocomplete
          v-model="value"
          :items="items"
          dense
          filled
          dark
          label="Search or select character"
        ></v-autocomplete>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "HelloWorld",

  data: () => ({
    items: ["foo", "bar", "fizz", "buzz"],
    values: ["foo", "bar"],
    value: null,
  }),
};
</script>

<style scoped>
v-text {
  color: rgb(255, 255, 255);
  padding-top: 100%;
  text-align: center;
  font-size: 20px;
}
</style>
