<template>
  <div id="piechart1"></div>
</template>

<script>
import * as d3Core from "d3";
import * as d3Collection from "d3-collection";

let d3;
d3 = Object.assign({}, d3Core, d3Collection);

/* eslint-disable */
export default {
  name: "ReadabilityPieChart",
  props: {
    data: {
      type: Object,
      default: () => ({}),
    },
    width: {
      type: Number,
      default: 0,
    },
    height: {
      type: Number,
      default: 0,
    },
    legendItemsPerLine: {
      type: Number,
      default: 1,
    },
  },
  computed: {
    keys() {
      return Object.keys(this.data);
    },
    chartStyle() {
      return {width: this.width + "px", height: this.height + "px"};
    },
  },
  mounted() {
    this.render();
  },
  methods: {
    render() {
// set the dimensions and margins of the graph
      var width = 150
      var height = 150
      var margin = 5

// The radius of the pieplot is half the width or half the height (smallest one). I subtract a bit of margin.
      var radius = Math.min(width, height) / 2 - margin

// append the svg object to the div called 'my_dataviz'
      var svg = d3.select("#piechart1")
          .append("svg")
          .attr("width", width)
          .attr("height", height)
          .append("g")
          .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");

// Create dummy data
      var data = {a: 2, b: 6}

// set the color scale
      var color = d3
          .scaleOrdinal()
          .domain(Object.keys(data)) //<-- set domain to ['a','b','c','d','e']
          .range(['#8AF3FF', '#F7F0F0']);

// Compute the position of each group on the pie:
      var pie = d3.pie()
          .value(function (d) {
            return d.value;
          })
      var data_ready = pie(d3.entries(data))

// Build the pie chart: Basically, each part of the pie is a path that we build using the arc function.
      svg
          .selectAll('whatever')
          .data(data_ready)
          .enter()
          .append('path')
          .attr('d', d3.arc()
              .innerRadius(50)         // This is the size of the donut hole
              .outerRadius(radius)
          )
          .attr('fill', function (d) {
            return (color(d.data.key))
          })
          .attr("stroke", "#bdbdbd")
          .style("stroke-width", "2px")
          .style("opacity", 0.7)


      svg.append("svg:text")
          .attr("dy", ".35em")
          .attr("text-anchor", "middle")
          .attr("style","font-family:Ubuntu")
          .attr("font-size","50")
          .text("📖");
    }
  }
}
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
  margin-top: 0;
  position: relative;
}


</style>