<template>
  <v-app>
    <v-main>
      <GameProcess />
    </v-main>
  </v-app>
</template>

<script>
import GameProcess from "./components/GameProcess";

export default {
  name: "App",

  components: {
    GameProcess,
  },

  data: () => ({
    //
  }),
};
</script>
