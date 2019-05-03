<template>
    <div class="wrapper">
        <side-bar>
            <template slot="links">
                <sidebar-link to="/" :name="'Dashboard'" icon="tim-icons icon-chart-pie-36"/>
                <sidebar-link v-for="station in stations" :key="station.id"
                        :to="`/station/${station.id}`" :name="station.name" icon="tim-icons icon-cloud-download-93"/>
            </template>
        </side-bar>
        <div class="main-panel">
            <top-navbar></top-navbar>

            <dashboard-content @click.native="toggleSidebar"></dashboard-content>

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
    import MobileMenu from "./MobileMenu";
    import axios from "axios";

    export default {
        components: {
            TopNavbar,
            ContentFooter,
            DashboardContent,
            MobileMenu
        },
        data: function() {
            return {
                stations: null
            }
        },
        methods: {
            toggleSidebar() {
                if (this.$sidebar.showSidebar) {
                    this.$sidebar.displaySidebar(false);
                }
            }
        },
        mounted () {
            if (!sessionStorage.stations) {
                axios
                    .get(process.env.VUE_APP_API_URL + 'v1/stations')
                    .then(function (response) {
                        if (response.data.success) {
                            sessionStorage.stations = JSON.stringify(response.data.data);
                            this.stations = JSON.parse(sessionStorage.stations);
                        }
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            } else {
                this.stations = JSON.parse(sessionStorage.stations);
            }
        }
    };
</script>
