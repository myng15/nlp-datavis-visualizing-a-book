<template>
  <h1> BookVisualizer </h1>
  <NetworkDiagram/>
</template>

<script>
import NetworkDiagram from "@/components/NetworkDiagram";

export default {
  name: 'App',
  components: {
    NetworkDiagram,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
