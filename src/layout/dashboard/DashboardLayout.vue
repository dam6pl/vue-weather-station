<template>
  <div class="wrapper">
    <!-- <side-bar :loader="loader">
            <template slot="links">
                <sidebar-link to="/" :name="'Dashboard'" icon="tim-icons icon-chart-pie-36"/>
                <sidebar-link v-for="station in stations" :key="station.id"
                        :to="`/station/${station.id}`" :name="station.name" icon="tim-icons icon-cloud-download-93"/>
            </template>
    </side-bar>-->
    <div class="main-panel">
      <dashboard-content></dashboard-content>

      <content-footer></content-footer>
    </div>
  </div>
</template>
<style lang="scss">
</style>
<script>
import TopNavbar from "./TopNavbar.vue";
import ContentFooter from "./ContentFooter.vue";
import DashboardContent from "./Content.vue";
import axios from "axios";

export default {
  components: {
    TopNavbar,
    ContentFooter,
    DashboardContent
  }
};
</script>
