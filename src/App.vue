<template>
  <QuizAnswerTable />
</template>

<script>
import QuizAnswerTable from "./components/QuizAnswerTable.vue";
export default {
  name: "App",
  components: {
    QuizAnswerTable,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background: #242424;
}

body {
  margin: 0;
  padding: 0;
  background: #242424;
}

.highlightColor {
  font-weight: 800;
  background-color: rgb(168 85 247);
  border: 1px solid rgb(233 213 255);
  padding: 2px;
}
</style>
