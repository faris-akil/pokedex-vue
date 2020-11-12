<template>
  <div>
    <v-container>
      <v-row class="justify-center text-center">
        <v-col cols="10">
          <v-text-field
            v-model="searchQuery"
            label="Press enter to start seaching, can be pokemon id or full name (e.g 1 or pikachu)"
            rounded
            outlined
            @keyup.enter="submitSearch"
          ></v-text-field>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: "",
    };
  },
  watch: {
    searchQuery: function(newValue){
      if (newValue === ''){
        this.$emit("searchResult", this.searchQuery);
      }
    }
  },
  methods: {
    submitSearch() {
      this.$emit("searchResult", this.searchQuery);
    },
  },
};
</script>

<style></style>
