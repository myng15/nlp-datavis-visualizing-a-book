<template>
  <div id="network">
  </div>
</template>

<script>

import * as d3 from "d3";
import cooccurrences from "@/data/cooccurrences_15characters.json";

export default {

  components: {},
  mounted() {
    this.init();
  },
  methods: {
    init() {

      const margin = {top: 2, right: 10, bottom: 10, left: 20},
          width = 500 - margin.left - margin.right,
          height = 500 - margin.top - margin.bottom;

      const svg = d3.select("#network")
          .append("svg")
          .attr("width", width + margin.left + margin.right)
          .attr("height", height + margin.top + margin.bottom)
          .append("g")
          .attr("transform",
              `translate(${margin.left}, ${margin.top})`);

      const data = cooccurrences;
      console.log("Data", data)
 
      /* eslint-disable */

      //Scales
      let color = d3.scaleOrdinal(d3.schemeCategory10);
      let nodeSize = d3.scaleLinear()
          .domain([0, 1000]) // unit: occurences 
          .range([5, 25]) // unit: pixels

      let simulation = d3.forceSimulation()
          .force("link", d3.forceLink().id(function(d) { return d.id; }))
          .force("charge", d3.forceManyBody().strength(-400))
          .force("center", d3.forceCenter(width / 2, height / 2));

      let link = svg.append("g")
          .attr("class", "links")
          .selectAll("line")
          .data(data.links)
          .enter().append("line")
          .attr("stroke-width", 2); //Define a scale for stroke width or stroke opacity!

      let node = svg.append("g")
          .attr("class", "nodes")
          .selectAll("g")
          .data(data.nodes)
          .enter().append("g");

      let circles = node.append("circle")
          .attr("r", d => nodeSize(d.size))
          .attr("fill", function(d) { return color(d.id)});

      
      let lables = node.append("text")
          .text(function(d) {
            return d.name;
          })
          .attr('x', 6)
          .attr('y', 3);

      node.append("title")
          .text(function(d) { return 'Whatever you want to show as tooltip!' });

      
      // Create a drag handler and append it to the node object instead
      let drag_handler = d3.drag()
          .on("start", dragstarted)
          .on("drag", dragged)
          .on("end", dragended);
      
      drag_handler(node);

      simulation
          .nodes(data.nodes)
          .on("tick", ticked);

      simulation.force("link")
          .links(data.links);

      function ticked() {
        link
            .attr("x1", function(d) { return d.source.x; })
            .attr("y1", function(d) { return d.source.y; })
            .attr("x2", function(d) { return d.target.x; })
            .attr("y2", function(d) { return d.target.y; });

        node
            .attr("transform", function(d) {
              return "translate(" + d.x + "," + d.y + ")";
            })
      }

    function dragstarted(event, d) {
      if (!event.active) simulation.alphaTarget(0.3).restart();
      d.fx = d.x;
      d.fy = d.y;
    }

    function dragged(event, d) {
      d.fx = event.x;
      d.fy = event.y;
    }

    function dragended(event, d) {
      if (!event.active) simulation.alphaTarget(0);
      d.fx = null;
      d.fy = null;
    }

    }
    /* eslint-enable */
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

.links line {
  stroke: #999;
  stroke-opacity: 0.6;
}

.nodes circle {
  stroke: #fff;
  stroke-width: 1.5px;
}

text {
  font-family: sans-serif;
  font-size: 10px;
}
</style>