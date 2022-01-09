<template>
  <v-app>
    <v-content>
      <router-view class="content"></router-view>
    </v-content>
  </v-app>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      //
    }
  }
}
</script>

<style lang="scss">
  html, head, body {
    margin: 0!important;
    padding: 0!important;
    display: block!important;
    overflow-y: hidden;
  }
  .content {
    display: block;
    margin: 0 auto;
    padding: 0;
    width: 100%;
  }
</style>
