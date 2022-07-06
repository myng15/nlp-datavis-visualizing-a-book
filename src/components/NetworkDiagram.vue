<template>
  <div id="network">
    <h3>Character Network</h3>
  </div>
</template>

<script>

import * as d3 from "d3";
import cooccurrences from "@/data/network/corrected_cooccurrences.json";

export default {

  components: {},
  mounted() {
    this.init();
  },
  methods: {

    init() {

      const margin = {top: 2, right: 10, bottom: 10, left: 20},
          width = 800 - margin.left - margin.right,
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


      console.log(data.links.index)
 
      /* eslint-disable */

      //Scales
      let colorNode = d3.scaleOrdinal(d3.schemeCategory10);
      let nodeSize = d3.scaleLinear()
          .domain([0, 1000]) // unit: occurences 
          .range([5, 25]) // unit: pixels



      let simulation = d3.forceSimulation()
          .force("link", d3.forceLink().id(function(d) { return d.id; }).distance(100).strength(1))
          .force("charge", d3.forceManyBody().strength(-1000))
          .force("center", d3.forceCenter(width/2 , height/2))


      let link = svg.append("g")
          .selectAll("line")
          .data(data.links)
          .enter().append("line")
          .attr("stroke", function(d) {
            return d.color;
          })
          .attr("stroke-width", 1);//Define a scale for stroke width or stroke opacity!

      let node = svg.append("g")
          .attr("class", "nodes")
          .selectAll("g")
          .data(data.nodes)
          .enter().append("g");

      let circles = node.append("circle")
          .attr("r", d => nodeSize(d.size))
          .attr("fill", function(d) { return colorNode(d.id)});

      
      let lables = node.append("text")
          .text(function(d) {
            return d.name;
          })
          .attr('x', 6)
          .attr('y', 3)
          .attr("fill", function(d) { return colorNode(d.id)})
          .attr("fontSize",20)

      node.append("title")
          .text(function(d) { return 'Whatever you want to show as tooltip!' });

      //node.on("click", click(function(d) { return d.text}))

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

        function click(name) {
         console.log(name)

        }
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

#network {
  font-family: 'PT Sans', sans-serif;
}

.title {
  position: absolute;
  text-align: center;
  left: 2em;
}

h1, a {
  color: #18A999;
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
  stroke-opacity: 0.5;
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