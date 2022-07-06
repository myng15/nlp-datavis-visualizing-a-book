<template>
 <div id="bubble-chart">
  <svg id="bubble-chart-svg">
    <g id="chartWrapper">
      <g class="grid-x"></g>
      <g class="grid-y"></g>
      <g id="bubbles"></g>
    </g>
  </svg>
 </div>
</template>

<script>

import * as d3 from "d3";
import termsData from "@/data/bubblechart/top_terms_arr_per_topic_7.json";

// import data from "@/data/topic_bubbles_data.json";
// const chartData = Object.values(data) //can be used interchangeably with data_chart from props

export default {
  props: { // all following props are typed (optional)
    data: Array,
    data_chart: Array,
    // colorScale: null,
    },
  data: function() {
    return {
      settings: {
        margin: { // just test values
          top: 20, 
          right: 40, 
          bottom: 30, 
          left: 40
        },
      width: 400,
      height: 267,
      opacityCircles: 0.8,
      // colorScale: d3.scaleOrdinal(d3.schemeSet2).domain(chartData)
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
      console.log(this.data_chart)
    this.svgContainer = d3
      // .select("#bubble-chart")
      // .append("svg")
      .select("#bubble-chart-svg")
      .attr("width", (this.settings.width + this.settings.margin.left + this.settings.margin.right))
      .attr("height", (this.settings.height + this.settings.margin.top + this.settings.margin.bottom))

    const xScale = d3.scaleLinear()
      .range([0, this.settings.width])
      .domain(this.key_dom(this.key_x))
    this.svgContainer.append("g")
      .attr("transform", "translate(" + this.settings.margin.left + "," + (this.settings.height + this.settings.margin.top) + ")")
      .call(d3.axisBottom(xScale))

    const yScale = d3.scaleLinear()
          .range([this.settings.height, 0])
          .domain(this.key_dom(this.key_y))
    this.svgContainer.append("g")
      .attr("transform", "translate(" + this.settings.margin.left + "," + this.settings.margin.top + ")")
      .call(d3.axisLeft(yScale));

    this.chartWrapper = this.svgContainer
          // .append("g")
          // .attr("class", "chartWrapper")
          .select("#chartWrapper")
          .attr("transform", "translate(" + this.settings.margin.left + "," + this.settings.margin.top + ")");

    this.chartWrapper
      // .append("g")
      //   .attr("class", "grid")
        .select(".grid-x")
        .attr("transform", "translate(0," + this.settings.height + ")")
        .call(d3.axisBottom(xScale)
            .ticks(5)
            .tickSize(-this.settings.height)
            .tickFormat("")
        )
    this.chartWrapper
        // .append("g")
        // .attr("class", "grid")
        .select(".grid-y")
        .call(d3.axisLeft(yScale)
            .ticks(5)
            .tickSize(-this.settings.width)
            .tickFormat("")
        )
    
    //Add axis labels   
    // this.chartWrapper.append("text")
    //       .attr("x", (this.settings.width / 2))
    //       .attr("y", this.settings.height + this.settings.margin.top)
    //       .attr("dy", "1em")
    //       .style("text-anchor", "middle")
    //       .text("x");

    // this.chartWrapper.append("text")
    //       .attr("transform", "rotate(-90)")
    //       .attr("y", 0 - this.settings.margin.left)
    //       .attr("x", 0 - (this.settings.height / 2))
    //       .attr("dy", "1em")
    //       .style("text-anchor", "middle")
    //       .text("y");

    const rScale = d3.scaleLinear()
			.range([15, 25])
      .domain(this.key_dom(this.key_r))

    const colorScale = d3.scaleOrdinal(d3.schemeTableau10).domain(this.data_chart)

    this.bubbles = 
      this.chartWrapper
      // .append('g')
      .select("#bubbles")
      .selectAll("dot")
      .data(this.data_chart)
      .enter()
      .append("circle")
        .attr("class", "bubble")
        .style("opacity", this.settings.opacityCircles)
        .style("fill", function(d) {return colorScale(d);})
        .style("stroke", "gray")
        .attr("cx", function(d) {return xScale(d.x);})
        .attr("cy", function(d) {return yScale(d.y);})
        .attr("r", function(d) {return rScale(d.count)})
       
    // Add bubble tooltips
    const tooltip = d3.select('#bubble-chart').append("div")
                      .attr("id", "bubble-tooltip");
    
    const handleMouseOver = (e, d) => { //e: MouseEvent, d: chart data
      const topTermsArray = termsData[this.data_chart.indexOf(d)].terms.slice(0, 10);
      const topTerms = topTermsArray.map(i => i.name).join(", ")
      d3.select("#bubble-tooltip")
              .style("left", e.pageX + "px")
              .style("top", e.pageY + "px")
              .style("display", "block")
              .style("background", "white")
              .style("box-shadow", "3px 3px 10px rgba(0, 0, 0, 0.4)")
              .style("border-radius", "5px")
              .html("Frequency (words): " + d.count 
                    + "<br>Top words: " + topTerms + ",...");
    }

    this.bubbles.on("mouseover", handleMouseOver)
           .on("mouseout", () => {
              tooltip.style("display", "none");
    });

    },
    key_dom: function(key){
      var dom = d3.extent(this.data_chart, function(d) { return parseFloat(d[key]) ; })  // d3.extent returns [min, max] in a single pass over the input --> used to set a scale's domain
      return dom
    }
  }
}
</script>

<style>
#bubble-tooltip {
    position: absolute;
    max-width: 200px;
    height: auto;
    padding: 5px;
    pointer-events: none;
    line-height: 20px;
    text-align: left;
  }
  
</style>