<template>
  <div class="wrapper">
    <fade-transition :duration="1000" mode="out-in">
      <div class="main-panel" v-if="showError">
        <div class="content" style="height: 1px;">
          <div class="row justify-content-center align-items-center h-100">
            <div class="col-12 col-md-6 text-center">
              <h1 class="title text-danger">404 Nie znaleziono</h1>
              <h2 class="title">Oops! Strona której szukasz wydaje się nie istnieć.</h2>
            </div>
          </div>
          <content-footer></content-footer>
        </div>
      </div>
    </fade-transition>
  </div>
</template>

<script>
import ContentFooter from "./../layout/dashboard/ContentFooter.vue";
import { FadeTransition } from "vue2-transitions";

export default {
  components: {
    ContentFooter,
    FadeTransition
  },
  data() {
    return {
      year: new Date().getFullYear(),
      version: false,
      versionText: "",
      showError: false
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
    this.showError = true;
  }
};
</script>
