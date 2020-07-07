<template>
  <div class="home">
    <h1>{{ message}}</h1>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome",
      places: [],
      errors: [],
      newPlaceName: "",
      newPlaceAddress: "",
      currentPlace: {}
    };
  },
  created: function() {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function() {
      axios.get("/api/places").then(response => {
        console.log("All Places:", response.data);
        this.places = response.data;
      });
    },
    createPlace: function() {
      var params = {
        name: this.newPlaceName,
        address: this.newPlaceAddress
      };
      axios
        .post("/api/places", params)
        .then(response => {
          console.log("Success", response.data);
          this.places.push(response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },
    showPlace: function(place) {
      console.log(place);
      this.currentPlace = place;
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function(place) {
      var params = {
        name: place.name,
        address: place.address
      };
      axios
        .patch(`/api/palces/${place.id}`, params)
        .then(response => {
          console.log("Successful update", response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
        });
    },
    destroyPlace: function(place) {
      axios.delete(`/api/palces/${place.id}`).then(response => {
        console.log("Successful delete", response.data);
        var index = this.places.indexOf(place);
        this.places.splice(index, 1);
      });
    }
  }
};
</script>
