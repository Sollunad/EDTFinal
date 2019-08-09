<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline">
        <span>The Eurovision Decade Tournament - Grand Final Results</span>
      </v-toolbar-title>
    </v-app-bar>

    <v-content>
      <ScoreboardComp></ScoreboardComp>
    </v-content>
  </v-app>
</template>

<script>
import ScoreboardComp from "./components/ScoreboardComp";

export default {
  name: 'App',
  components: {
    ScoreboardComp,
  },
  data: () => ({
    //
  }),
};
</script>
