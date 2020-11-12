<template>
  <div>
    <v-container fluid>
      <v-row v-if="searchUrl === ''" dense>
        <v-col
          v-for="pokemon in pokemons"
          :key="pokemon.title"
          cols="12"
          sm="6"
          md="3"
        >
          <v-card
            @click="openPokemonDetail(pokemon.url)"
            min-width="200px"
            max-height="200px"
            color="blue-grey lighten-4"
          >
            <v-img
              :src="`${imageUrl}/${pokemon.id}.png`"
              class="white--text align-end"
              contain
              height="100"
            >
            </v-img>
            <v-card-title
              class="black--text text-body-1 justify-center text-uppercase"
              >#{{ pokemon.id }} {{ pokemon.name }}</v-card-title
            >
          </v-card>
        </v-col>
        <v-col cols="12" class="text-center my-2" v-if="loading">
          <div>
            <v-progress-circular indeterminate color="red"></v-progress-circular>
          </div>
        </v-col>
      </v-row>
      <v-row v-if="searchUrl !== ''"> 
        <v-col offset-md="2" md="8">
          <span v-if="!notFound">
            
          <v-card
            @click="openPokemonDetail(`${apiUrl}${searchResult.id}`)"
            min-width="200px"
            max-height="200px"
            color="blue-grey lighten-4"
          >
            <v-img
              :src="`${searchResult.sprites.other['official-artwork']['front_default']}`"
              class="white--text align-end"
              contain
              height="100"
            >
            </v-img>
            <v-card-title
              class="black--text text-body-1 justify-center text-uppercase"
              >#{{ searchResult.id }} {{ searchResult.name }}</v-card-title
            >
          </v-card>
          </span>
          <span v-if="notFound">
            <v-alert border="right" colored-border type="error" elevation="2">No pokemon found</v-alert>
          </span>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  props: ["imageUrl", "apiUrl", "searchUrl"],
  data() {
    return {
      pokemons: [],
      nextUrl: "",
      currentUrl: "",
      notFound: false,
      searchResult: '',
      loading: false
    };
  },
  watch: {
    searchUrl: function(newVal) {
      if(newVal === ''){
        this.pokemons = []
        this.fetchData()
      } else {
        this.searchPokemon(newVal)
      }
    },
  },
  methods: {
    async fetchData() {
      this.pokemons.length === 0 ? this.currentUrl = this.apiUrl : this.currentUrl;
      let req = await fetch(this.currentUrl);
      let data = await req.json();
      this.nextUrl = data.next;
      data.results.forEach((pokemon) => {
        this.pokemons.push({
          id: pokemon.url.split("/").reverse()[1],
          ...pokemon,
        });
      });
      this.loading = false
    },
    scroll(){
      window.onscroll = () => {
        this.loading = true;
         let bottomOfWindow = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight;
         if(this.nextUrl && bottomOfWindow){
           this.next();
         }
      }
    },
    scrollTrigger() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.intersectionRatio > 0 && this.nextUrl) {
            this.next();
          }
        });
      });
      observer.observe(this.$refs.infinitescrolltrigger);
    },
    next() {
      this.currentUrl = this.nextUrl;
      this.fetchData();
    },
    openPokemonDetail(url) {
      this.$emit("getPokemonDetail", url);
    },
    async searchPokemon(url) {
      try {
        let req = await fetch(this.apiUrl + url);
        let data = await req.json();
        console.log(data);
        this.notFound = false;
        this.searchResult = data
        console.log(Object.keys(this.searchResult.sprites.other))
        console.log(this.searchResult.sprites.other["official-artwork"]["front_default"])
        // {{searchResult.sprites.other[official-artwork][front_default]}}
      } catch (error) {
        this.notFound = true
        // console.log(error)
      }
    },
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  },
  mounted() {
    this.scroll();
    // this.scrollTrigger();
  },
};
</script>

<style></style>
