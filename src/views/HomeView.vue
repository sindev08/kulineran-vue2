<template>
  <div class="home">
    <NavBar />
    <div class="container">
      <HeroView />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import NavBar from "@/components/Navbar.vue";
import HeroView from "@/components/Hero.vue";

export default {
  name: "HomeView",
  components: {
    NavBar,
    HeroView,
  },
};
</script>
