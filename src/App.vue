<template>
  <div id="app">
    <div id="header">
      <div id="div1">
        <img :src="anne" :width="150" :height="80"/>
      </div>
      <div id="div2">
        <H1>BookVisualizer - Anne of Green Gables </H1>
      </div>
    </div>
    <div id = "main1">
      <h1> general info following soon </h1>
      <BarChart></BarChart>
<!--      <div class="arrow">
        <div class="line"></div>
        <div class="point"></div>
      </div>-->
    </div>
  <div id="main2">
   <NetworkDiagram v-on:changeCharacter="characterChange($event)"></NetworkDiagram>
   <WordCloudCharacter :characterKey="characterKey" @changeCharacter="characterChange"></WordCloudCharacter>
  </div>
  <div id="main3">
    <div id="submain3">
      <div id="submain3-buttons">
        <button
          class="toggle"
          @click="toggle"
          :class="[showBubbleChart ? 'btn-active' : 'btn-inactive']"
        >Intertopic Distance Map</button>
        <button
          class="toggle"
          @click="toggle"
          :class="[!showBubbleChart ? 'btn-active' : 'btn-inactive']"
        >Top Terms per Topic</button>
      </div>
      <div id="bubble-chart-container" :class="[showBubbleChart ? 'chart-active' : 'chart-inactive']">
        <BubbleChart :chapterKey="chapterKey" @changeChapter="chapterChange" v-on:topic="getTopic($event)"></BubbleChart>
      </div>
      <div id="circle-pack-container" :class="[!showBubbleChart ? 'chart-active' : 'chart-inactive']">
        <CirclePack :chapterKey="chapterKey" @changeChapter="chapterChange" @changeTopicFromCirclePack="getTopic($event)"></CirclePack>
      </div>
    </div>
    <WordCloudChapter v-on:changeChapter="chapterChange($event)" :topicKey="topicKey" @topic="getTopic"></WordCloudChapter>
  </div>
  </div>
</template>

<script>
import NetworkDiagram from "@/components/NetworkDiagram";
import BarChart from "@/components/BarChart";
import WordCloudChapter from "@/components/WordCloudChapter";
import BubbleChart from "@/components/BubbleChart";
import CirclePack from "./components/CirclePack.vue";
import WordCloudCharacter from "@/components/WordCloudCharacter";
//import ReadabilityPieChart from "@/components/ReadabilityPieChart";

export default {
  name: 'App',
  data() {
    return {
      anne: require('./assets/anne.svg'),
      showBubbleChart: true,
      chapterKey: "",
      characterKey: "",
      topicKey: ""
    }
  },
  components: {
    WordCloudChapter,
    BarChart,
    BubbleChart,
    NetworkDiagram,
    CirclePack,
    WordCloudCharacter,
   // ReadabilityPieChart

},
  methods: {
    toggle(e) {
      if (!e.target.classList.contains('btn-active')){
        this.showBubbleChart = !this.showBubbleChart;
      }
    },
    chapterChange(event){
      this.chapterKey = event;
    },
    characterChange(event){
      this.characterKey=event;
    },
    getTopic(event){
      this.topicKey=event;
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=PT+Sans');

body {
  font-family: 'PT Sans', sans-serif;
  background-color: #eee;
}

#app {
  background: #F7F0F0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #18A999;
  margin-top: 2px;
}

#main1 {
  padding: 5px;
  display: grid;
  row-gap: 5px;
  row-gap: 5px;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 5px;
  grid-auto-rows: minmax(50px, auto);
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
  row-gap: 5px;
  row-gap: 5px;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 5px;
  grid-auto-rows: minmax(100px, auto);
  grid-column: 2/2;
  grid-row: 1;
}


#submain3{
  margin-bottom: 5px;
}

#submain3-buttons button {
  border: 1px solid #bbc1be; 
  font-size: 16px;
  font-weight: bold; 
  padding: 10px 24px; 
  cursor: pointer; 
  transition: 0.3s;
  box-shadow: 5px 5px 10px rgba(0,0,0,0.05);
}

#submain3-buttons button:first-child {
  border-radius: 5px 0 0 5px;
}

#submain3-buttons button:last-child {
  border-radius: 0 5px 5px 0;
}

#submain3-buttons button:not(:last-child) {
  border-right: none; 
}

#submain3-buttons button:hover{
  background-color: #18A999; /* #3e8e41; */
  color: white;
}

#bubble-chart-container {
  padding: 5px 0;
}

.btn-active {
  background-color: #18A999;/* #3e8e41; */
  color: white;
}

.btn-inactive {
  background-color: white;
  color: #18A999; /* #3e8e41; */
}

.chart-active {
  display: block;
}

.chart-inactive {
  display: none;
}

#div1 {
  background: #109648;
  padding: 2px;
  display: inline-block;
  width: auto;
  height: 75px;
  justify-content:left;
  float:left;
}

#div2 {
  background: #109648;
  padding: 2px;
  width: auto;
  height: 75px;
  display: inline-block;
  flex-direction: column;
  justify-content: left;
  text-align: center;
  line-height: 50px;
}

#header{
  padding: 5px;
  background: #109648;
}
h1 {
  color: #F7F0F0;
  text-align:center;
}

.arrow {
  width: 120px;
  position:relative;
}

.line {
  margin-top: 14px;
  width: 90px;
  background: blue;
  height: 10px;
  float: left;
}

.point {
  width: 0;
  height: 0;
  border-top: 20px solid transparent;
  border-bottom: 20px solid transparent;
  border-left: 30px solid blue;
  float: right;
}

</style>