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
      var width = 125
      var height = 125
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
          .range(['#109648','#F7F0F0']);

// Compute the position of each group on the pie:
      var pie = d3.pie()
          .value(function (d) {
            return d.value;
          }).sort(null);
      var data_ready = pie(d3.entries(data))

// Build the pie chart: Basically, each part of the pie is a path that we build using the arc function.
      svg
          .selectAll('whatever')
          .data(data_ready)
          .enter()
          .append('path')
          .attr('d', d3.arc()
              .innerRadius(45)         // This is the size of the donut hole
              .outerRadius(radius)
          )
          .attr('fill', function (d) {
            return (color(d.data.key))
          })
          .attr("stroke", "#bdbdbd")
          .style("stroke-width", "2px")
          .style("opacity", 0.7)
          .style("cursor", function(d) {return d.value === 2 ? "pointer" : "default"})


      svg.append("svg:text")
          .attr("dy", ".35em")
          .attr("text-anchor", "middle")
          .attr("style","font-family:Ubuntu")
          .attr("font-size","65")
          .text("📖");

      //Add  tooltips
      const tooltip = d3.select('#piechart1').append("div")
          .attr("id", "piechart-tooltip");

      const handleMouseOver = (e, data) => {
        d3.select("#piechart-tooltip")
            .style("left", e.pageX + "px")
            .style("top", e.pageY + "px")
            .attr('text-anchor', 'middle')
            // .style("position", "absolute")
            .style("display", "block")
            .style("background", "white")
            .style("box-shadow", "3px 3px 10px rgba(0, 0, 0, 0.4)")
            .style("border-radius", "5px")
            .html("Readability Score: " + data.value + "/8\n(Suitable for 6th and 7th grade)");
      }
      
      d3.select('#piechart1').selectAll("path").on("mouseover", (e,d) => {if(d.value === 2) {handleMouseOver(e, d)}})
          .on("mouseout", () => {
            tooltip.style("display", "none");
          });
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

#piechart-tooltip {
  position: absolute;
  max-width: 120px;
  height: auto;
  padding: 4px;
  pointer-events: none;
  text-align: left;
  line-height: 16px;
  font-size: 12.5px;
}


</style>