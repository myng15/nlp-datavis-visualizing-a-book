<template>
  <div id="bubble-chart-legends"></div>
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
  mounted: function() {
    // Add word count legends
    const svg = d3.select("#bubble-chart-legends")
      .append("svg")
      .attr("width", (this.width))
      .attr("height", (this.height))
      .attr("transform", "translate(" + this.settings.width*1.22 + "," + + (this.settings.height - this.settings.margin.top - this.settings.margin.bottom - 20) + ")");
  
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
      .attr('y', function(d){ return valuesToShow.indexOf(d) === 1 ? yCircle - rScale(d) - 5 : valuesToShow.indexOf(d) === 2 ? yCircle - rScale(d) - 10 
                                                                      : yCircle - rScale(d) } )
      .text( function(d){ return d } )
      .style("font-size", "10px")
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