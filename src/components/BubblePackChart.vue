<template>
    <div id="bubble-pack">
      <BubbleChart
      :data="data"
      :data_chart="data_chart"
      :colorScale="colorScale"
      ></BubbleChart>   
    </div>
</template>

<script>
import * as d3 from "d3";
// eslint-disable-next-line no-unused-vars
import dataset from "@/data/topic_bubbles_data.json";

import BubbleChart from '@/components/BubbleChart.vue'

export default {
  components: {
    BubbleChart
  },
  data() {
    return {
      data: [],
      data_chart: [],
      legend_class: [],
      colorScale: null,
    }
  },
  created: function() {
    var that = this 

    d3.json("'/src/data/top_terms_per_topic.json'",
      function(data) {  
        that.data.push(data)
      }
    );
    this.data_chart = this.data
    this.colorScale = d3.scaleOrdinal(d3.schemeSet3)
  }
}
</script>

<style>

</style>
