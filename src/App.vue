<template>
  <v-app>
   
    <!-- <Navbar /> -->
    <v-main>
      <Pokemon />
    </v-main>
  </v-app>
</template>

<script>
// import Navbar from "./components/Navbar"
import Pokemon from './components/Pokemon';

export default {
  name: 'App',

  components: {
    Pokemon,
    // Navbar
  },

  data: () => ({
    //
  }),
};
</script>
