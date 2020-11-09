<template>
  <div>
    <v-dialog v-model="dialog" max-width="390">
      <template v-if="pokemon.name">
        <v-card>
          <v-card-title class="headline justify-center">
            <v-avatar size="200">
              <v-img
                :src="
                  pokemon['sprites']['other']['official-artwork'][
                    'front_default'
                  ]
                "
                contain
              /> </v-avatar
          ></v-card-title>
          <v-card-text class="text-body-1 text-capitalize text-center">{{
            pokemon.name
          }}</v-card-text>
          <v-card-text class="d-inline-flex justify-space-around">
            <div>Height: {{pokemon.height}} ft</div>
            <div>Weight: {{pokemon.weight}} lbs</div>
          </v-card-text>
          <v-card-text>
              Pokemon Type : <span v-for="(value, index) in pokemon.types" :key="index">
                                <v-chip :color="checkType(value)" outlined class="ml-1">{{value.type.name}}</v-chip> 
                             </span>
          </v-card-text>
          <v-card-text>   
              Abilities : <span v-for="ability in pokemon.abilities" :key="ability.id">
                            <v-chip outlined class="ml-1">{{ability.ability.name}}</v-chip>
                          </span>
          </v-card-text>
        </v-card>
      </template>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: ["pokemonUrl", "imageUrl", "showDetail"],
  data() {
    return {
      pokemon: {},
      dialog: this.showDetail ? true : false,
    };
  },
  watch: {
    dialog(val) {
      if (!val) this.closeDialog();
    },
  },
  methods: {
    closeDialog() {
      this.$emit("closeDialog", false);
    },
    async fetchData() {
      let req = await fetch(this.pokemonUrl);
      let data = await req.json();
      this.pokemon = data;
      console.log(data);
    },
    checkType(val){
        console.log(val.type.name)
        let color;
        switch (val.type.name) {
            case "fire":
                color = "red";
                break;
            case "normal":
                color = "grey";
                break;
            case "water":
                color = "blue";
                break;
            case "grass":
                color = "green";
                break;
            case "flying":
                color = "silver";
                break;
            case "fighting":
                color = "brown";
                break;
            case "poison":
                color = "purple";
                break;
            case "electric":
                color = "yellow";
                break;
            case "ground":
                color = "red";
                break;
            case "rock":
                color = "red";
                break;
            case "ice":
                color = "red";
                break;
            case "bug":
                color = "red";
                break;
            default:
                color = "black"
                break;
        }
        return color
    }
  },
  created() {
    this.fetchData();
  },
};
</script>

<style></style>
