<template>
  <div>
     <v-container fluid>
      <v-row dense>
        <v-col
          v-for="pokemon in pokemons"
          :key="pokemon.title"
          :cols="3"
        >
          <v-card @click="openPokemonDetail(pokemon.url)">
            <v-img
              :src="`${imageUrl}/${pokemon.id}.png`"
              class="white--text align-end"
              contain
              height="100"
            >
            </v-img>
                <v-card-title class="black--text text-body-1 justify-center text-uppercase">#{{pokemon.id}}  {{pokemon.name}}</v-card-title>
          </v-card>
        </v-col>
      </v-row>
      <div class="text-center mt-10" ref="infinitescrolltrigger">
        <v-progress-circular
          indeterminate
          color="red"
        ></v-progress-circular>
      </div>

    </v-container>

  </div>
</template>

<script>
export default {
  props: ["imageUrl", "apiUrl"],
  data() {
    return {
      pokemons: [],
      nextUrl: "",
      currentUrl: "",
    };
  },
  methods: {
    async fetchData() {
      let req = await fetch(this.currentUrl);
      let data = await req.json();
      this.nextUrl = data.next
      data.results.forEach((pokemon) => {
        this.pokemons.push({
          id: pokemon.url.split("/").reverse()[1],
          ...pokemon
        }) 
      });  
    },
    scrollTrigger(){
      const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if(entry.intersectionRatio > 0 && this.nextUrl){
            this.next()
          }
        })
      })
      observer.observe(this.$refs.infinitescrolltrigger)
    },
    next(){
      this.currentUrl = this.nextUrl;
      this.fetchData();
    },
    openPokemonDetail(url){
      this.$emit('getPokemonDetail', url)
    }
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  },
  mounted(){
    this.scrollTrigger();
  }
};
</script>

<style></style>
