<template>
 <v-app id="app">
    <user-page />
  </v-app>
</template>

<script>
import UserPage from './components/userPage.vue'

export default {
  name: 'App',
  components: {
    UserPage
  }
}
</script>

<style>
@import url("https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css");
</style>