<template>
  <div class="container" style="padding: 50px 0 100px 0">
    <LandingPage v-if="user" />
    <Login v-else />
  </div>
</template>
<script setup lang="js">
import LandingPage from "./pages/LandingPage.vue";
import Login from "./pages/Login.vue";

const user = useSupabaseUser();
</script>
