<template>
  <section>
    <ShoppingList />
  </section>
</template>

<script>
import ShoppingListVue from "./components/ShoppingList.vue";
export default {
  name: "App",
  components: {
    ShoppingList: ShoppingListVue,
  },
  title () {
    return `Shopping List App`
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

body {
  font-size: 16px;
  margin: 0;
  height: 100%;
  overflow: hidden;
}
</style>
