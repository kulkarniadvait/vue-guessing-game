<template>
  <v-app>
    <v-app-bar app color="primary" dark> </v-app-bar>

    <v-main>
      <Game />
    </v-main>
  </v-app>
</template>

<script>
import Game from "./components/Game.vue";

export default {
  name: "App",

  components: {
    Game,
  },
};
</script>
