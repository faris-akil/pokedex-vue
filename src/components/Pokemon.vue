<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <h2 class="display-5 font-weight-bold mb-3">
          Pokemon
        </h2>
      </v-col>
    </v-row>
    <PokemonSearch />
    <PokemonList
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
    pokemonUrl: "",
  }),
  methods: {
    setPokemonDetail(url) {
      console.log("run");
      this.pokemonUrl = url;
      this.showDetail = true;
    },
    closeDialog() {
      this.showDetail = false;
      this.pokemonUrl = "";
    },
  },
  beforeMount() {},
};
</script>
