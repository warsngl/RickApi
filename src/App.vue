<script>
import TheNavbar from "./components/TheNavbar.vue";
import Main from "./views/Main.vue";
export default {
  data: () => ({
    name: "",
    status: "",
  }),
  methods: {
    filterBy(filter) {
      this.name = filter.name;
      this.status = filter.status;
    },
  },
  components: { TheNavbar, Main },
};
</script>

<template lang='pug'>
TheNavbar.mb-8(@filter="filterBy")
Main(:filterByName="name", :filterByStatus="status")
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #f5f5f5;
  min-width: 100vw;
  min-height: 100vh;
  padding: 20px;
  background: #272b33;
}
</style>
