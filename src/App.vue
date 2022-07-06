<template>
  <div id="app">
    <div id="header">
      <div id="div1">
        <img :src="anne" :width="150" :height="150"/>
      </div>
      <div id="div2">
        <H1>BookVisualizer - Anne of Green Gables </H1>
      </div>
    </div>
    <div id = "main1">
      <div> general info following soon</div>
      <BarChart></BarChart>
    </div>
  <div id="main2">
    <NetworkDiagram></NetworkDiagram>
  </div>
  <div id="main3">
    <BubbleChart></BubbleChart>
    <WordCloudChapter></WordCloudChapter>
  </div>
  </div>
</template>

<script>
import NetworkDiagram from "@/components/NetworkDiagram";
import BarChart from "@/components/BarChart";
import WordCloudChapter from "@/components/WordCloudChapter";
import BubbleChart from "@/components/BubbleChart";

export default {
  name: 'App',
  data() {
    return {
      anne: require('./assets/anne.svg')
    }
  },
  components: {
    WordCloudChapter,
    BarChart,
    BubbleChart, NetworkDiagram
  }
}
</script>

<style>
#app {
  background: #F7F0F0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #18A999;
  margin-top: 10px;
}

#main1 {
  padding: 15px;
  display: grid;
  row-gap: 10px;
  row-gap: 10px;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 10px;
  grid-auto-rows: minmax(100px, auto);
  grid-column: 2 / 2;
  grid-row: 1;
}

#main2 {
  display: grid;
  row-gap: 10px;
  row-gap: 10px;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 10px;
  grid-auto-rows: minmax(100px, auto);
  grid-column: 2/2;
  grid-row: 1;
}

#main3 {
  display: grid;
  row-gap: 10px;
  row-gap: 10px;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 10px;
  grid-auto-rows: minmax(100px, auto);
  grid-column: 2/2;
  grid-row: 1;
}

#div1 {
  background: #109648;
  padding: 2px;
  display: inline-block;
  width: auto;
  height: 150px;
  justify-content:left;
  float:left;
}

#div2 {
  background: #109648;
  padding: 2px;
  width: auto;
  height: 150px;
  display: inline-block;
  flex-direction: column;
  justify-content: left;
  text-align: center;
  line-height: 100px;
}

#header{
  padding: 10px;
  background: #109648;
}
h1 {
  color: #F7F0F0;
  text-align:center;
}
</style>