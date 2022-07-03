<template>
  <div id="barchart">
  </div>
</template>

<script>

import * as d3 from "d3";
import Anne from "@/data/barcharts/Anne Shirley.json";
import Diana from "@/data/barcharts/Diana Barry.json";
import Marilla from "@/data/barcharts/Marilla Cuthbert.json";
import Matthew from "@/data/barcharts/Matthew Cuthbert.json";
import Rachel from "@/data/barcharts/Rachel Lynde.json";
import Allan from "@/data/barcharts/Allan Anne.json";
import Carmody from "@/data/barcharts/Carmody.json";
import Gilbert from "@/data/barcharts/Gilbert Blythe.json";
import Jane from "@/data/barcharts/Jane Andrews.json";
import Josie from "@/data/barcharts/Josie Pye.json";
import Phillips from "@/data/barcharts/Phillips.json";
import Ruby from "@/data/barcharts/Ruby Gillis.json";
import Spencer from "@/data/barcharts/Spencer.json";
import Stacy from "@/data/barcharts/Stacy.json";
import Thomas from "@/data/barcharts/Thomas Lynde.json";

export default {
  components: {},
  mounted() {
    this.init(Anne, "Anne Shirley");
    this.init(Diana, "Diana Barry");
    this.init(Marilla, "Marilla Cuthbert");
    this.init(Matthew, "Matthew Cuthbert");
    this.init(Rachel, "Rachel Lynde");
    this.init(Allan, "Allan Anne");
    this.init(Carmody, "Carmody");
    this.init(Gilbert, "Gilbert Blythe");
    this.init(Jane, "Jane Andrews");
    this.init(Josie, "Josie Pye");
    this.init(Phillips, "Phillips");
    this.init(Ruby, "Ruby Gillis");
    this.init(Spencer, "Spencer");
    this.init(Stacy, "Stacy");
    this.init(Thomas, "Thomas Lynde");
  },
  methods: {
    init(data, name) {
      // set the dimensions and margins of the graph
      var margin = {top:0 , right: 0, bottom: 0, left: 100},
          width = 460 - margin.left - margin.right,
          height = 20 - margin.top - margin.bottom;

// append the svg object to the body of the page
      var svg = d3.select("#barchart")
          .append("div")
          .append("svg")
          .attr("style", "outline: solid #D3D3D3")
          .attr("width", width + margin.left + margin.right)
          .attr("height", height + margin.top + margin.bottom)
          .append("g")
          .attr("transform",
              "translate(" + margin.left + "," + margin.top + ")")

// X axis
      var x = d3.scaleBand()
          .range([20, width])
          .domain(data.map(function (d) {
            return d.Segment;
          }))
          .padding(0.1);
      svg.append("g")
        /*  .attr("transform", "translate(0," + height + ")")
          .call(d3.axisBottom(x))
          .selectAll("text")
          .attr("transform", "translate(-10,0)rotate(-45)")
          .style("text-anchor", "end")
          .selectAll("text").remove();*/

//Getting max value to set length of y-axis
      var maxValue = Math.max(...data.map(o => o.Value))
      console.log(maxValue);


// Add Y axis
      var y = d3.scaleLinear()
          .domain([0, maxValue])
          .range([height, 0]);
      svg.append("g")
      svg.append("text")
          .style("fill", "#696969")
          .style("font-size", "12px")
          .attr("class", "y label")
          .attr("text-anchor", "end")
          .attr("y", 2)
          .attr("dy", "1em")
         // .attr("transform", "rotate(-90)")
          .text(name);
          /*.call(d3.axisLeft(y))
          .selectAll("text").remove();*/

// Bars
      svg.selectAll("mybar")
          .data(data)
          .enter()
          .append("rect")
          .attr("x", function (d) {
            return x(d.Segment);
          })
          .attr("y", function (d) {
            return y(d.Value);
          })
          .attr("width", x.bandwidth())
          .attr("height", function (d) {
            return height - y(d.Value);
          })
          .attr("fill", "#69b3a2")

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
  margin-top: 0;
  position: relative;
}
</style>