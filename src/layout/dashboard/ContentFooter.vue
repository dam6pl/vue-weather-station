<template>
  <footer class="footer">
    <div class="container-fluid">
      <div class="copyright">
        © {{year}} utworzone z
        <i class="tim-icons icon-heart-2"></i> przez
        <a href="https://dnowak.dev" target="_blank" rel="noopener">Damian Nowak</a>
        <span v-if="version">{{versionText}}</span>
      </div>
    </div>
  </footer>
</template>
<script>
export default {
  data() {
    return {
      year: new Date().getFullYear(),
      version: false,
      versionText: ""
    };
  },
  mounted() {
    if (
      typeof process.env.VUE_APP_HEROKU_RELEASE_CREATED_AT !== "undefined" &&
      typeof process.env.VUE_APP_HEROKU_RELEASE_VERSION !== "undefined" &&
      process.env.VUE_APP_HEROKU_RELEASE_CREATED_AT !== "undefined" &&
      process.env.VUE_APP_HEROKU_RELEASE_VERSION !== "undefined"
    ) {
      let created_at = VUE_APP_HEROKU_RELEASE_CREATED_AT.replace("-", "")
        .replace("T", ".")
        .replace(":", "")
        .replace("Z", "");
      this.version = true;
      this.versionText = ` | ${process.env.VUE_APP_HEROKU_RELEASE_VERSION}.${created_at}`;
    }
  }
};
</script>
<style>
</style>
