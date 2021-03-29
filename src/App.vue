<template>
  <div id="app">
    <Sidebar class="sidebar"/>
    <Home class="home"/>
  </div>
</template>

<script>
import Sidebar from "./components/Sidebar.vue";
import Home from "./components/Home.vue";

export default {
  name: "App",
  components: {
    Sidebar,
    Home
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
.sidebar {
  float: left;
}
@media only screen and (max-width: 1000px) {
  .sidebar {
    clear: both;
  }
  .home {
    clear: both;
  }
}
</style>
