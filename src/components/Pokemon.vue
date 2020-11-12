<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <h2 class="display-5 font-weight-bold my-2 text-decoration-underline">
          PokeDex Vue
        </h2>
      </v-col>
    </v-row>
    <PokemonSearch @searchResult="getSearchQuery"/>
    <PokemonList
      :searchUrl="searchUrl"
      :imageUrl="imageUrl"
      :apiUrl="apiUrl"
      @getPokemonDetail="setPokemonDetail"
    />
    <PokemonDetail
      v-if="showDetail"
      :showDetail="showDetail"
      :pokemonUrl="pokemonUrl"
      :imageUrl="imageUrl"
      @closeDialog="closeDialog"
    />
  </v-container>
</template>

<script>
import PokemonList from "@/components/PokemonList";
import PokemonDetail from "@/components/PokemonDetail";
import PokemonSearch from "@/components/PokemonSearch"

export default {
  name: "Pokemon",
  components: {
    PokemonList,
    PokemonDetail,
    PokemonSearch
  },
  data: () => ({
    imageUrl:
      "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/",
    apiUrl: "https://pokeapi.co/api/v2/pokemon/",
    showDetail: false,
    searchUrl: '',
    pokemonUrl: "",
  }),
  methods: {
    setPokemonDetail(url) {
      this.pokemonUrl = url;
      this.showDetail = true;
    },
    closeDialog() {
      this.showDetail = false;
      this.pokemonUrl = "";
    },
    getSearchQuery(value){
      this.searchUrl = value;
    }
  },
  beforeMount() {},
};
</script>
