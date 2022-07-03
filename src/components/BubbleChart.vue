<template>
 <div id="bubble-chart">
  <h3>Topic distribution</h3>
  <CirclePack></CirclePack>
 </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
import * as d3 from "d3";
// eslint-disable-next-line no-unused-vars
// import data from "@/data/topic_bubbles_data.json";

import CirclePack from '@/components/CirclePack.vue'
export default {
  components: { CirclePack },
  props: { // all following props are typed (optional)
    data: Array,
    data_chart: Array,
    colorScale: null,
    },
  data: function() {
    return {
      settings: {
        margin: { // just test values
          top: 20, 
          right: 20, 
          bottom: 20, 
          left: 20
        },
      width: 600,
      height: 400,
      },
      svgContainer: Object,
      chartWrapper: Object,
      key_x: "x", //x-coordinate of bubble
      key_y: "y", //y-coordinate of bubble
      key_r: "count", //count of terms in corpus that belong to bubble
    }
  },
   mounted() {
    this.init();
  },
  methods: {
    init() {
    this.svgContainer = d3.select("#bubble-chart")
      .append("svg")
      .attr("width", (this.settings.width + this.settings.margin.left + this.settings.margin.right))
      .attr("height", (this.settings.height + this.settings.margin.top + this.settings.margin.bottom))

    
    },
    key_dom: function(key){
      var dom = d3.extent(this.data, function(d) { return parseFloat(d[key]) ; })  // d3.extent returns [min, max] in a single pass over the input --> used to set a scale's domain
      return dom
    }
  }
}
</script>

<style>

</style>