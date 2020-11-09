<template>
  <div>
    <v-row class="d-flex justify-center text-center">
      <v-col cols="6">
        <v-text-field
          v-model="searchQuery"
          label="Search Pokemon..."
          clearable
          rounded
          outlined
          @keyup="submitSearch"
        ></v-text-field>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
    data(){
        return {
            searchQuery: ""
        }
    },
    methods: {
        submitSearch(){
            console.log(this.searchQuery)
        }
    }
};
</script>

<style></style>
