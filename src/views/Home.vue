<template>
  <v-container>
    <h1 class="text-center">Nice Café Brew!</h1>
    <h3 class="text-center">Find your highest-rated local café brew today.</h3>
    <v-form style="max-width: 500px;" class="mx-auto">
      <v-text-field
        solo
        label="Location"
        v-model="location"
        prepend-inner-icon="mdi-map-marker"
        append-outer-icon="mdi-send"
        @click:append-outer="doSearch"
      ></v-text-field>
    </v-form>
  </v-container>
</template>

<script>
  export default {
    name: 'Home',
    data() {
      return {
        location: '',
      }
    }, 
    methods: {
      doSearch() {
        
      }
    }
  }
</script>

<style lang="less" scoped>


h1, h2, h3, h4, h5 {
  font-family: 'Roboto', cursive;
  color: #555;
  font-weight: 300;
  margin: 2rem 0;
}
h1 {
  font-size: 4rem;
  font-weight: bold;
}
</style>