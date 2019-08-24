<template>
  <div class="row justify-content-center align-items-center h-100">
    <div>
      <h2>Wybierz lokalizację stacji:</h2>
      <div class="lds-ellipsis" v-if="loader">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
      </div>
      <ul v-if="!loader" class="stations-list">
        <li v-for="station in stations" :key="station.id">
          <a
            :href="`/station/${station.id}`"
          >{{ station.name }} ({{ station.latitude }} {{ station.longitude }})</a>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  data: function() {
    return {
      stations: null,
      loader: true
    };
  },
  mounted() {
    axios
      .get(process.env.VUE_APP_API_URL + "v1/stations")
      .then(response => {
        if (response.data.success) {
          this.stations = response.data.data;
          this.loader = !this.loader;
        }
      })
      .catch(error => {
        console.log(error);
        this.$notify({
          type: "danger",
          message: "Błąd strony: " + error.message
        });
      });
  }
};
</script>
<style>
</style>
