<template>
  <div id="app">
    <div id="header">
      <div id="div1" >
      <img :src="book" />
      </div>
      <div id="div2" >
      <H1>BookVisualizer</H1>
      </div>
      </div>
    <div id="main1">
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
      book: require('./assets/book.svg'),
      barChartData: [
          {
            name: "Roses",
            amount: 25
          },
          {
            name: "Tulips",
            amount: 40
          },
          {
            name: "Daisies",
            amount: 15
          },
          {
            name: "Narcissuses",
            amount: 9
          }
        ]
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
  margin-top: 10px;
}

#main1{
  display: grid;
  row-gap: 10px;
  row-gap: 10px;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 10px;
  grid-auto-rows: minmax(100px, auto);
  grid-column: 4 / 4;
  grid-row: 2;
}

#div1 {
  padding: 10px;
  display: inline-block;
  width:120px;
  height:120px;
}
#div2 {
  padding: 10px;
  display: inline-block;
  width:120px;
  height:120px;
}
</style>
