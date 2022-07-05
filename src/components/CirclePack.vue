<template>
  <div id="circle-pack"></div>
</template>

<script>
import * as d3 from "d3";
import data from "@/data/bubblechart/top_terms_per_topic.json";
// eslint-disable-next-line no-unused-vars
const dataset = {...data['topic 1'].terms}
export default {
  data: () => ({
    width:      904,
    height:     904,
  }),
  computed: {
    
  },
  watch: {
    
  },
  mounted() {
    this.init()
    // Assign Sizes
    // this.updateSize()
    
    // const data = await d3.json('/data/top_terms_per_topic.json')
    // // Assign raw data as read only to $dataset object
    // this.dataset = Object.freeze(data.terms)
  },
  methods: {
    init() {

      // eslint-disable-next-line no-unused-vars
      const svg = d3.select("#circle-pack")
          .append("svg")
          .attr("viewBox", `-${this.width / 2} -${this.height / 2} ${this.width} ${this.height}`)
          .style("display", "block")
        
    
    },
    
  }
}
</script>

<style>

</style>