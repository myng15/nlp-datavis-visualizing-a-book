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
    <NetworkDiagram v-on:changeCharacter="characterChange($event)"></NetworkDiagram>
<!--    <WordCloudCharacter :characterKey="characterKey" @changeCharacter="characterChange"></WordCloudCharacter>-->
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
        >Top terms per topic</button>
      </div>
      <div id="bubble-chart-container" :class="[showBubbleChart ? 'chart-active' : 'chart-inactive']">
        <BubbleChart :chapterKey="chapterKey" @changeChapter="chapterChange"></BubbleChart>
      </div>
      <div id="circle-pack-container" :class="[!showBubbleChart ? 'chart-active' : 'chart-inactive']">
        <CirclePack></CirclePack>
      </div>
    </div>
    <WordCloudChapter :chapterKey="chapterKey"></WordCloudChapter>
  </div>
  </div>
</template>

<script>
import NetworkDiagram from "@/components/NetworkDiagram";
import BarChart from "@/components/BarChart";
import WordCloudChapter from "@/components/WordCloudChapter";
import BubbleChart from "@/components/BubbleChart";
import CirclePack from "./components/CirclePack.vue";
//import WordCloudCharacter from "@/components/WordCloudCharacter";

export default {
  name: 'App',
  data() {
    return {
      anne: require('./assets/anne.svg'),
      showBubbleChart: true,
      chapterKey: "",
      characterKey:""
    }
  },
  components: {
    WordCloudChapter,
    BarChart,
    BubbleChart,
    NetworkDiagram,
    CirclePack,
    //WordCloudCharacter
},
  methods: {
    toggle(e) {
      if (!e.target.classList.contains('btn-active')){
        this.showBubbleChart = !this.showBubbleChart;
      }
    },
    chapterChange(event){
      this.chapterKey = event;
      console.log(event)
    },
    characterChange(event){
      this.characterKey=event;
      console.log(event)
    }
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

#submain3{
  margin-bottom: 10px;
}

#submain3-buttons button {
  /* background-color: #18A999;  */
  border: 1px solid green; 
  color: white;
  font-weight: bold; 
  padding: 10px 24px; 
  cursor: pointer; 
  transition: 0.3s;
}

#submain3-buttons button:not(:last-child) {
  border-right: none; /* Prevent double borders */
}

/* #submain3-buttons:after {
  content: "";
  clear: both;
  display: table;
} */

#submain3-buttons button:hover{
  background-color: #3e8e41;
}

#bubble-chart-container,
#circle-pack-container {
  padding: 20px;
}

.btn-active {
  background-color: #3e8e41;
}

.btn-inactive {
  background-color: #18A999;
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