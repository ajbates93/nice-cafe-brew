<template>
  <v-container>
    <h1 class="text-center">Nice Café Brew!</h1>
    <h3 class="text-center">Find your highest-rated local café brew today.</h3>
    <v-form style="max-width: 500px;" class="mx-auto">
      <v-autocomplete
        solo
        label="Location"
        :loading="this.isLoading"
        v-model="searchInput"
        :search-input.sync="search"
        cache-items
        hide-no-data
        prepend-inner-icon="mdi-map-marker"
        append-outer-icon="mdi-send"
        @click:append-outer="doSearch()"
        :items="this.searchResults"
        return-object
      ></v-autocomplete>
    </v-form>
    <div id="results">
      <template v-for="(place, idx) in places">
        <v-card :key="idx">
          <v-card-title>{{place.name}}</v-card-title>
        </v-card>
      </template>
    </div>
  </v-container>
</template>

<script>
  export default {
    name: 'Home',
    data() {
      return {
        searchInput: null,
        search: null,
        searchResults: [],
        selectedSearchResult: null,
        service: null,
        placesService: null,
        geocoder: null,
        geocodeResults: {
          lat: '',
          lng: ''
        },
        isLoading: false,
        selectedCoordinates: null,
        selectedCoordinateLat: null,
        selectedCoordinateLng: null,
        places: []
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
      MapsInit () {
        this.service = new window.google.maps.places.AutocompleteService()
        this.placesService = new window.google.maps.places.PlacesService()
        this.geocoder = new window.google.maps.Geocoder()
      },
      doSearch() {
        this.isLoading = true
        this.placesService.getDetails(this.selectedSearchResult[0].place_id, function(place, status) {
          if (status == 'OK') {
            console.log(place.geometry.location)
            this.selectedCoordinates = place.geometry.location
          }
        })
        // this.geocoder.geocode({
        //   'placeId': this.selectedSearchResult[0].place_id
        // }, function(responses, status) {
        //   console.log(responses)
        //   this.selectedCoordinates = responses[0].geometry.location
        //   if (status == 'OK') {
        //     this.isLoading = false
        //   } else {
        //     console.log('error: ' + status)
        //   }
        // })
      },
      displaySuggestions (predictions, status) {
        if (status !== window.google.maps.places.PlacesServiceStatus.OK) {
          this.searchResults = []
          this.isLoading = false
          return
        }
        this.searchResults = predictions.map(prediction => prediction.description)
        this.selectedSearchResult = predictions.map(prediction => prediction)
        // this.selectedCoordinates = predictions.map(prediction => {
        //   this.geocoder.geocode({
        //     'placeId': prediction.place_id
        //   }, function(responses, status) {
        //     if (status == 'OK') {
        //       return {lat: responses[0].geometry.location.lat(), lng: responses[0].geometry.location.lng()}
        //     }
        //   })
        // })
        this.isLoading = false
      }
    },
    watch: {
      search (newValue) {
        if (newValue) {
          this.isLoading = true
          this.service.getPlacePredictions({
            input: this.search,
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