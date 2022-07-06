<template>
  <div id="bubble-chart-legends">
    <ul id="bubble-legends">
      <li
          v-for="(input, index) in legend_class"
          :key="`input-${index}`"
      >
        <div id="legendElementWrapper"
             @click="sendFilterInput(input, index)"
        >
          <svg id="rectLegend" width="20" height="20">
            <!-- <rect :style="cssRectFill(input)" width="20" height="20"/> -->
            <rect id="rectLegend2" width="20" height="20"/>
          </svg>
          {{ input }}
        </div>

      </li>
    </ul>
  </div>
</template>

<script>
import * as d3 from "d3";
// import data from "@/data/topic_bubbles_data.json";
// const chartData = Object.values(data)

export default {
  props: {
    data: Array,
    data_chart: Array
  },
  data: function () {
    return {
      legend_class: [],
      clickInput: [],
      width: 100,
      height: 333,
      key_r: "count"
    }
  },
  created: function () {
    this.legend_class = this.data_chart.map(a => `Topic ${this.data_chart.indexOf(a) + 1}`)
    // this.legend_class = chartData.map(a => `Topic ${chartData.indexOf(a)+1}`)
    // this.legend_class = this.data.map(a => `Topic ${this.data.indexOf(a)+1}`)
    this.clickInput = new Array(this.legend_class.length).fill(false)
  },
  mounted: function () {
    // Add topic legends
    const colorScale = d3.scaleOrdinal(d3.schemeTableau10).domain(this.data_chart);
    d3.selectAll("#legendElementWrapper").select("#rectLegend").select("#rectLegend2").data(this.data_chart).style("fill", function (d) {
      return colorScale(d);
    })

    // Add word count legends
    this.svg = d3.select("#bubble-chart-legends")
        .append("svg")
        .attr("width", (this.width))
        .attr("height", (this.height))

    const rScale = d3.scaleLinear()
        .range([10, 20])
        .domain(this.key_dom(this.key_r))

    const valuesToShow = [500, 1000, 1500, 2000]
    const xCircle = 30
    const xLabel = 40
    this.svg
        .data(valuesToShow)
        .enter()
        .append("circle")
        .attr("cx", xCircle)
        .attr("cy", function (d) {
          return this.height - rScale(d)
        })
        .attr("r", function (d) {
          return rScale(d)
        })
        .style("fill", "none")
        .attr("stroke", "black")

    this.svg
        .data(valuesToShow)
        .enter()
        .append("line")
        .attr('x1', function (d) {
          return xCircle + rScale(d)
        })
        .attr('x2', xLabel)
        .attr('y1', function (d) {
          return this.height - rScale(d)
        })
        .attr('y2', function (d) {
          return this.height - rScale(d)
        })
        .attr('stroke', 'black')
        .style('stroke-dasharray', ('2,2'))
  },
  methods: {
    key_dom: function (key) {
      var dom = d3.extent(this.data_chart, function (d) {
        return parseFloat(d[key]);
      })  // d3.extent returns [min, max] in a single pass over the input --> used to set a scale's domain
      return dom
    },
    sendFilterInput(input, index) {
      if (this.clickInput[index] == false) {
        this.clickInput[index] = true
        this.$emit('inputChange', input)
      } else if (this.clickInput[index] == true) {
        this.clickInput[index] = false
        this.$emit('inputChangeBack', input)
      }
    },
    cssRectFill(legItem) {
      // const colorScale = d3.scaleOrdinal(d3.schemeTableau10).domain(chartData);
      const colorScale = d3.scaleOrdinal(d3.schemeTableau10).domain(this.data_chart);
      return {
        '--fill': colorScale(legItem)
      }
    }
  },
  computed: {}
}
</script>

<style>
ul {
  text-align: left;
}

ul li {
  list-style: none;
}

#legendElementWrapper {
  cursor: pointer;
}

</style>