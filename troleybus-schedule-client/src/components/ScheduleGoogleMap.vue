<template>
  <div>
    <div class="map">
      <label>
        <gmap-autocomplete
          @place_changed="setPlace">
        </gmap-autocomplete>
        <button @click="addMarker">Add</button>
      </label>
      <gmap-map
        :center="center"
        :zoom="12"
        style="width:100%;  height: 400px;"
      >
        <gmap-marker
          :key="index"
          v-for="(m, index) in markers"
          :position="m.position"
          @click="center=m.position"
        ></gmap-marker>
      </gmap-map>
    </div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        // default to Montreal to keep it simple
        // change this to whatever makes sense
        center: {lat: 45.508, lng: -73.587},
        markers: [],
        places: [],
        currentPlace: null,
        items: []
      }
    },
    mounted () {
      this.geolocate()
    },
    methods: {
      setPlace (place) {
        this.currentPlace = place
      },
      addMarker () {
        if (this.currentPlace) {
          const marker = {
            lat: this.currentPlace.geometry.location.lat(),
            lng: this.currentPlace.geometry.location.lng()
          }
          this.markers.push({position: marker})
          this.places.push(this.currentPlace)
          this.center = marker
          this.currentPlace = null
        }
      },
      geolocate: function () {
        navigator.geolocation.getCurrentPosition(position => {
          this.center = {
            lat: position.coords.latitude,
            lng: position.coords.longitude
          }
        })
      }
    }
  }
</script>
<style>
  .map {
    margin-left: 1000px;
  }
</style>
