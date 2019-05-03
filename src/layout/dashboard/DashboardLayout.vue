<template>
    <div class="wrapper">
        <side-bar>
            <template slot="links">
                {{this.stations}}
                <sidebar-link to="/dashboard" :name="'Dashboard'" icon="tim-icons icon-chart-pie-36"/>
                <sidebar-link to="/station/1" :name="'Stacja Bobowa'" icon="tim-icons icon-cloud-download-93"/>
                <sidebar-link to="/station/2" :name="'Stacja Nowy Sącz'" icon="tim-icons icon-cloud-download-93"/>
            </template>
        </side-bar>
        <div class="main-panel">
<!--            <top-navbar></top-navbar>-->

            <dashboard-content @click.native="toggleSidebar">

            </dashboard-content>

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
            axios
                .get(process.env.VUE_APP_API_URL + 'v1/stations/')
                .then(response => (this.stations = response))
        }
    };
</script>
