<template>
  <div id="app">
    <AppHeader />
    <RestaurantList />
    <AppFooter />
  </div>
</template>
<script>
import RestaurantList from "./components/RestaurantList.vue";
import AppHeader from "./components/Header.vue";  
import AppFooter from "./components/Footer.vue";  
export default {
  name: "App",
  components: {
    RestaurantList,
    AppHeader,  
    AppFooter,  
  },
};
</script>
<style>
body {
  font-family: Arial, sans-serif;
  margin: 20px;
}
h1 {
  color: #333;
  text-align: center;
}
</style>
