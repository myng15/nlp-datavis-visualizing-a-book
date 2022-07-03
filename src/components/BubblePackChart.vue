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
/* eslint-disable */
import * as d3 from "d3";
import topics from "@/data/bubblechart/top_terms_per_topic.json";

import dataset from "@/data/bubblechart/topic_bubbles_data.json";

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
    this.data_chart = topics
    //this.colorScale = d3.scaleOrdinal(d3.schemeSet3)
  }
}

/* eslint-enable */
</script>

<style>

</style>
