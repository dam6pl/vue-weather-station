<template>
  <div class="wrapper">
    <div class="main-panel">
      <top-navbar></top-navbar>

      <div class="content" style="height: 1px;">
        <div class="row justify-content-center align-items-center h-100">
          <div class="text-center">
            <h1 class="title text-danger">404 Nie znaleziono</h1>
            <h2 class="title">Oops! Strona której szukasz wydaje się nie istnieć.</h2>
          </div>
        </div>
        <content-footer></content-footer>
      </div>
    </div>
  </div>
</template>

<script>
import TopNavbar from "./../layout/dashboard/TopNavbar";
import ContentFooter from "./../layout/dashboard/ContentFooter.vue";

export default {
  components: {
    TopNavbar,
    ContentFooter
  },
  data() {
    return {
      year: new Date().getFullYear(),
      version: false,
      versionText: ""
    };
  },
  mounted() {
    if (
      typeof process.env.VUE_APP_HEROKU_SLUG_COMMIT !== "undefined" &&
      typeof process.env.VUE_APP_HEROKU_RELEASE_VERSION !== "undefined"
    ) {
      this.version = true;
      this.versionText = ` | ${process.env.VUE_APP_HEROKU_SLUG_COMMIT}${process.env.VUE_APP_HEROKU_RELEASE_VERSION}`;
    }
  }
};
</script>
