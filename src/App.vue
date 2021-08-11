<template>
  <v-app>
    <v-app-bar app color="green" dark>
      <div class="d-flex align-center">
        <v-icon size="40">mdi-food-apple</v-icon>
        <h1>Alimentos</h1>
      </div>
    </v-app-bar>
    <v-main>
      <v-container>
        <router-view />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  }),
};
</script>
