<template>
  <div id="app">
    <div id="header">
      <div id="div1">
      <img :src="book" />
      </div>
      <div id="div2">
      <H1>BookVisualizer</H1>
      </div>
    </div>
    <div id="main">
      <NetworkDiagram></NetworkDiagram>
      <WordCloud></WordCloud>
    </div>
  </div>
</template>

<script>
import NetworkDiagram from "@/components/NetworkDiagram";
import WordCloud from "@/components/WordCloud";

export default {
  name: 'App',
  data() {
    return {
      book: require('./assets/book.svg')
    }
  },
  components: {
    NetworkDiagram,
    WordCloud
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

#header {
}
#div1 {
  display: inline-block;
  width:120px;
  height:120px;
}
#div2 {
  display: inline-block;
  width:120px;
  height:120px;
}
</style>
