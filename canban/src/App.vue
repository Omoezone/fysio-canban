<template>
  <div id="app" style="height: 5rem;">
    <Kanban />
  </div>
</template>

<script>
import Kanban from "./components/kanban-board.vue";

export default {
  name: "App",
  components: {
    Kanban,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 2rem;
}
</style>
