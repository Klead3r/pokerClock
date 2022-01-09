<template>
  <div>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Poker</span>
        <span class="font-weight-light">clock</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        flat
      >
        <router-link to="/">Home</router-link>
      </v-btn>
      <v-btn
        flat
      >
        <router-link to="tournamentclock">Tournament clock</router-link>
      </v-btn>
    </v-toolbar>
  </div>
</template>

<script>
  export default {
  }
</script>
