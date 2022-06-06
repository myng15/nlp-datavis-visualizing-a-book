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

      const margin = {top: 10, right: 30, bottom: 30, left: 40},
          width = 400 - margin.left - margin.right,
          height = 400 - margin.top - margin.bottom;

      const svg = d3.select("#network")
          .append("svg")
          .attr("width", width + margin.left + margin.right)
          .attr("height", height + margin.top + margin.bottom)
          .append("g")
          .attr("transform",
              `translate(${margin.left}, ${margin.top})`);

      const data = cooccurrences;

      console.log(data.links)

      /* eslint-disable */
      // Initialize the links
      const link = svg
          .selectAll("line")
          .data(data.links)
          .join("line")
          .style("stroke", "#aaa")

      /* eslint-disable */
      const node = svg
          .selectAll("circle")
          .data(data.nodes)
          .join("circle")
          .attr("r", 20)
          .style("fill", "#69b3a2")

      /* eslint-disable */
      const label = svg
          .selectAll("circle")
          .data(data.nodes)
          .append("text")
          .attr("fill","black")
          .text(function(data) {
            return data.name;
          })
      /* eslint-enable */

      /* eslint-disable */
      // Let's list the force we wanna apply on the network
      var simulation = d3.forceSimulation(data.nodes)                 // Force algorithm is applied to data.nodes
          .force("link", d3.forceLink()                               // This force provides links between nodes
              .id(function (d) {
                return d.id;
              })                     // This provide  the id of a node
              .links(data.links)                                    // and this the list of links
          )
          .force("charge", d3.forceManyBody().strength(-400))         // This adds repulsion between nodes. Play with the -400 for the repulsion strength
          .force("center", d3.forceCenter(width / 2, height / 2))     // This force attracts nodes to the center of the svg area
          .on("end", ticked);


      // This function is run at each iteration of the force algorithm, updating the nodes position.
      function ticked() {
        link
            .attr("x1", function (d) {
              return d.source.x;
            })
            .attr("y1", function (d) {
              return d.source.y;
            })
            .attr("x2", function (d) {
              return d.target.x;
            })
            .attr("y2", function (d) {
              return d.target.y;
            });

        node
            .attr("cx", function (d) {
              return d.x + 6;
            })
            .attr("cy", function (d) {
              return d.y - 6;
            });
        /* eslint-enable */
      }

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