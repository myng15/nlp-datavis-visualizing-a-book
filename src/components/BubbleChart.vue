<template>

    <div id="bubble-pack">
      <div id="main-chart">
        <BubbleChartMain
          :data="data"
          :data_chart="data_chart"
          :key="chartReloadKey"
        />
      </div>
      <BubbleChartLegend 
        :data="data"
        :data_chart="data_chart"
        :legend_class="legend_class"
        @inputChange="filterInput"
        @inputChangeBack="filterInputBack"
      />
    
    </div>
</template>

<script>
// import * as d3 from "d3";

import data from "@/data/bubblechart/topic_bubbles_data_7.json";
// const chartData = Object.values(data)

import BubbleChartMain from '@/components/BubbleChartMain.vue'
import BubbleChartLegend from '@/components/BubbleChartLegend.vue'

export default {
  props: {
    chapterKey: Number
  },
  components: {
    BubbleChartMain,
    BubbleChartLegend
  },
  data() {
    return {
      data: [],
      data_chart: [],
      legend_class: [],
      chartReloadKey: 0,
      // colorScale: null,
    }
  },
  created: function() {
    var that = this 
    that.data = Object.values(data)
    // await d3.json("/src/data/topic_bubbles_data.json",
    //   function(data) {  
    //     that.data.push(data)
    //   }
    // );
    this.data_chart = this.data
    // this.data_chart = chartData
    // this.colorScale = d3.scaleOrdinal(d3.schemeSet3)
  },
  watch: {
    chapterKey: {
      deep: true,
      handler() {
        console.log(this.chapterKey)
      }
    }
  },
  methods: {
    filterInput (input) {
      // console.log(input)
      // console.log(`Topic ${this.data_chart.indexOf(this.data_chart[0])+1}` == input)
      this.data_chart = this.data_chart.filter(function(d){return `Topic ${this.data_chart.indexOf(d)+1}` != input;})
      this.chartReloadKey += 1
    },
    filterInputBack (input) {
      // console.log(input)
      // console.log(`Topic ${this.data_chart.indexOf(this.data_chart[0])+1}` == input)
      this.data_chart = this.data_chart.concat(this.data.filter(function(d){return `Topic ${this.data.indexOf(d)+1}` == input;}))
      this.chartReloadKey += 1
    },
  }
}
</script>

<style>
  #bubble-pack {
    display: flex;
    flex-direction: row;
  }
</style>
