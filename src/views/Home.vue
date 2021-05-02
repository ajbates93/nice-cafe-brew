<template>
  <v-container>
    <h1 class="text-center">Nice Café Brew!</h1>
    <h3 class="text-center">Find your highest-rated local café brew today.</h3>
    <v-form style="max-width: 500px;" class="mx-auto">
      <v-autocomplete
        solo
        label="Location"
        v-model="location"
        :search-input.sync="location"
        cache-items
        hide-no-data
        prepend-inner-icon="mdi-map-marker"
        append-outer-icon="mdi-send"
        @click:append-outer="this.doSearch()"
        :items="this.searchResults"
        return-object
      ></v-autocomplete>
    </v-form>
  </v-container>
</template>

<script>
  export default {
    name: 'Home',
    data() {
      return {
        location: null,
        searchResults: [],
        service: null,
      }
    },
    metaInfo () {
      return {
        script: [{
          src: `https://maps.googleapis.com/maps/api/js?key=AIzaSyDR8R7oq0mbgInPkBx2ZU8zcwdJ9GBb7oo&libraries=places`,
          async: true,
          defer: true,
          callback: () => this.MapsInit() // will declare it in methods
        }]
      }
    }, 
    methods: {
      doSearch() {
        
      },
      MapsInit () {
        this.service = new window.google.maps.places.AutocompleteService()
      },
      displaySuggestions (predictions, status) {
        if (status !== window.google.maps.places.PlacesServiceStatus.OK) {
          this.searchResults = []
          return
        }
        this.searchResults = predictions.map(prediction => prediction.description) 
      }
    },
    watch: {
      location (newValue) {
        if (newValue) {
          this.service.getPlacePredictions({
            input: this.location,
            types: ['(cities)']
          }, this.displaySuggestions)
        }
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