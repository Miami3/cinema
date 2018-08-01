<template>
  <v-container fluid>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>
        <h1>Online booking for Awesome Cinema</h1>
        <CinemaComponent></CinemaComponent>
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<script>
  import CinemaComponent from '../components/CinemaComponent';
  export default {
      name: 'Home',
      data () {
          return {

          }
      },
      components: { CinemaComponent }
  }
</script>
<style scoped>

</style>
