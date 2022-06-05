<template>
  <div id="network">
  </div>
</template>

<script>

import * as d3 from "d3";
import cooccurrences from "@/data/cooccurrences.json";

export default {
  components: {},
  mounted() {
    this.init();
  },
  methods: {
    init() {
      //We are accessing the div with the id chart using d3's select method and appending svg
      const svg = d3
          .select("#network")
          .append("svg")
          .attr("width", 450)
          .attr("height", 450)


      const data = cooccurrences;
      const node = svg.append("g")
          .selectAll("nodes")
          .data(data.nodes)
          .enter();
      /* eslint-disable */
      const circles = node.append("circle")
          .attr("cx", 40)
          .attr("cy", 40)
          .attr("r", 40)
      /* eslint-enable */

      const link = svg.append("g")
          .selectAll("links")
          .data(data.links)
          .enter();
      /* eslint-disable */
      const links = link.append("line")
          .style("stroke", "lightgreen")
          .style("stroke-width", 10);
      /* eslint-enable */
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=PT+Sans');

body {
  font-family: 'PT Sans', sans-serif;
  background-color: #eee;
}

.title {
  position: absolute;
  text-align: center;
  left: 2em;
}

h1, a {
  color: #1aad8d;
  text-decoration: none;
}

ul.menu {
  list-style: none;
  position: absolute;
  z-index: 100;
  min-width: 20em;
  text-align: left;
}

ul.menu li {
  margin-top: 1em;
  position: relative;
}
</style>