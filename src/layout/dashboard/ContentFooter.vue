<template>
  <footer class="footer">
    <div class="container-fluid">
      <div class="copyright">
        © {{year}} utworzone z <i class="tim-icons icon-heart-2"></i> przez
        <a href="https://dnowak.dev" target="_blank" rel="noopener">Damian Nowak</a>
      </div>
    </div>
  </footer>
</template>
<script>
  export default {
    data() {
      return {
        year: new Date().getFullYear()
      }
    }
  };
</script>
<style>
</style>
