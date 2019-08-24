<template>
  <div class="content" :class="this.$router.currentRoute.name == 'dashboard' ? 'full' : ''">
    <fade-transition :duration="1000" mode="out-in">
      <!-- your content here -->
      <router-view></router-view>
    </fade-transition>
  </div>
</template>
<script>
import { FadeTransition } from "vue2-transitions";
export default {
  components: {
    FadeTransition
  }
};
</script>
