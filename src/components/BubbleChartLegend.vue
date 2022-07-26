<template>
  <div id="bubble-chart-legends">
    <!-- //For adding topic legends:
    <ul id="bubble-legends">
      <li
          v-for="(input, index) in legend_class"
          :key="`input-${index}`"
      >
        <div id="legendElementWrapper"
        >
          <svg id="rectLegend" width="15" height="15">
            <rect id="rectLegend2" width="15" height="15"/>
          </svg>
          {{ input }}
        </div>
      </li>
    </ul> -->
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  props: {
    data: Array,
    data_chart: Array,
    settings: Object
  },
  data: function () {
    return {
      legend_class: [],
      width: 150,
      height: 150,
      key_r: "count"
    }
  },
  created: function() {
    // For adding topic legends:
    // const topicNames = {
    //   1: "Friends & love", 
    //   2:"School activities", 
    //   3: "Fashion & shows", 
    //   4: "Anne's emotional world", 
    //   5: "Life at Green Gables", 
    //   6: "Incidents with the Barrys", 
    //   7: "Mistakes & apologies"
    // }
    // this.legend_class = this.data_chart.map(a => topicNames[this.data_chart.indexOf(a)+1])
  },
  mounted: function() {
    // Add topic legends
    // const colorScale = d3.scaleOrdinal(d3.schemeTableau10).domain(this.data_chart);
    // d3.selectAll("#legendElementWrapper").select("svg").select("rect").data(this.data_chart).style("fill", function(d) {return colorScale(d);})

    // Add word count legends
    const svg = d3.select("#bubble-chart-legends")
      .append("svg")
      .attr("width", (this.width))
      .attr("height", (this.height))
      .attr("transform", "translate(" + this.settings.width*1.15 + "," + + (this.settings.height - this.settings.margin.top - this.settings.margin.bottom - 20) + ")");
      // .attr("transform", "translate(" + this.settings.margin.left*-.8 + "," + + (this.settings.height - this.settings.margin.top - this.settings.margin.bottom - 20) + ")");

    const rScale = d3.scaleLinear()
        .range([15,25])
        .domain(this.key_dom(this.key_r))  

    const valuesToShow = [1000, 1500, 2000]
    const xCircle = 70
    const xLabel = 115
    const yCircle = 100
    svg
      .selectAll("legend")
      .data(valuesToShow)
      .enter()
      .append("circle")
        .attr("cx", xCircle)
        .attr("cy", function(d){ return yCircle - rScale(d) } )
        .attr("r", function(d){ return rScale(d) })
        .style("fill", "none")
        .attr("stroke", "#18A999")

    svg
      .selectAll("legend")
      .data(valuesToShow)
      .enter()
      .append("line")
        .attr('x1', function(d){ return xCircle + rScale(d) } )
        .attr('x2', xLabel)
        .attr('y1', function(d){ return valuesToShow.indexOf(d) === 1 ? yCircle - rScale(d) - 5 : valuesToShow.indexOf(d) === 2 ? yCircle - rScale(d) - 10 
                                                                      : yCircle - rScale(d) } )
        .attr('y2', function(d){ return valuesToShow.indexOf(d) === 1 ? yCircle - rScale(d) - 5 : valuesToShow.indexOf(d) === 2 ? yCircle - rScale(d) - 10 
                                                                      : yCircle - rScale(d) } )
        .attr('stroke', '#18A999')
        .style('stroke-dasharray', ('2,2'))

    svg
      .selectAll("legend")
      .data(valuesToShow)
      .enter()
      .append("text")
      .attr('x', xLabel)
      .attr('y', function(d){ return valuesToShow.indexOf(d) === 1 ? yCircle - rScale(d) - 5 : valuesToShow.indexOf(d) === 2 ? yCircle - rScale(d) - 12 
                                                                      : yCircle - rScale(d) } )
      .text( function(d){ return d } )
      .style("font-size", 11)
      .attr('alignment-baseline', 'middle')
      .style('fill', '#18A999')
  },
  methods: {
    key_dom: function(key){
      var dom = d3.extent(this.data_chart, function(d) { return parseFloat(d[key]) ; })  // d3.extent returns [min, max] in a single pass over the input --> used to set a scale's domain
      return dom
    },
    
  },
  computed: {
  }
}
</script>

<style>
</style>