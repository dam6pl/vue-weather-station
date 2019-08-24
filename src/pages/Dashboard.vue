<template>
  <div class="row justify-content-center align-items-center h-100">
    <fade-transition :duration="1000" mode="out-in">
      <div class="col-12 col-md-6" v-if="!loader">
        <h2 class="text-center">Wybierz lokalizację stacji:</h2>
        <ul class="stations-list">
          <li v-for="station in stations" :key="station.id">
            <a
              :href="`/station/${station.id}`"
            >{{ station.name }} ({{ station.latitude }} {{ station.longitude }})</a>
          </li>
        </ul>
      </div>
    </fade-transition>
    <div class="col-12 col-md-6" v-if="loader">
      <div class="lds-ellipsis" v-if="loader">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import { FadeTransition } from "vue2-transitions";

export default {
  components: {
    FadeTransition
  },
  data: function() {
    return {
      stations: null,
      loader: true
    };
  },
  mounted() {
    this.showContent = true;
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
