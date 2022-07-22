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
    settings: Object,
    chapterKey: String
    },
  data: function() {
    return {
      // settings: {
      //   margin: { 
      //     top: 20, 
      //     right: 40, 
      //     bottom: 60, 
      //     left: 80
      //   },
      // width: 400,
      // height: 267,
      // opacityCircles: 0.9,
      // // colorScale: d3.scaleOrdinal(d3.schemeSet2).domain(chartData)
      // },
      svgContainer: Object,
      chartWrapper: Object,
      key_x: "x", //x-coordinate of bubble
      key_y: "y", //y-coordinate of bubble
      key_r: "count", //count of terms in corpus that belong to bubble
      topicKey: ""
    }
  },
  watch: {
    chapterKey: {
      deep: true,
      handler() {
        const chapter = parseInt(this.chapterKey);
        
        // If word cloud for the entire book is being shown:
        if(chapter === 0) {
          termsData.forEach(topic => d3.select(`#topic-${topic.topic}`)
                                                       .transition()
                                                       .duration(300)
                                                       .style("opacity", this.settings.opacityCircles))
        } else {
            const matchTopic = termsData.find(topic => topic.chapters.includes(chapter)).topic;

            const noMatchTopics = termsData.filter(topic => !topic.chapters.includes(chapter));
            d3.select(`#topic-${matchTopic}`)
              .transition()
              .duration(300)
              .style("opacity", this.settings.opacityCircles);

            noMatchTopics.forEach(topic => d3.select(`#topic-${topic.topic}`)
                        .transition()
                        .duration(300)
                        .style("opacity", 0.3))
        }
      }
    }
  },
  mounted() {
    this.init(); 
  },
  methods: {
    onChange(event, data) {
      d3.select(`#topic-${data}`)
        .style("opacity", this.settings.opacityCircles);
      const noMatchTopics = termsData.filter(topic => topic.topic !== data);
      noMatchTopics.forEach(topic => d3.select(`#topic-${topic.topic}`)
                                       .style("opacity", 0.3))
      
      // Sync with Circle Packs
      d3.select(`#topicPack-${data}`)
        .style("opacity", this.opacityCircles);
      noMatchTopics.forEach(topic => d3.select(`#topicPack-${topic.topic}`)
                                       .style("opacity", 0.3))

      this.$emit("changeTopic", data);
    },
    init() {
    this.svgContainer = d3
      // .select("#bubble-chart")
      // .append("svg")
      .select("#bubble-chart-svg")
      .attr("width", (this.settings.width + this.settings.margin.left + this.settings.margin.right + 50))
      .attr("height", (this.settings.height + this.settings.margin.top + this.settings.margin.bottom))
      .style("border-bottom", "1px solid #18A999")
      .style("border-left", "1px solid #18A999")
      .style("margin", "30px")

    const xScale = d3.scaleLinear()
      .range([0, this.settings.width])
      .domain(this.key_dom(this.key_x))
    // Add x-axis
    // this.svgContainer.append("g")
    //   .attr("transform", "translate(" + this.settings.margin.left + "," + (this.settings.height + this.settings.margin.top) + ")")
    //   .call(d3.axisBottom(xScale).tickFormat(()=>{return ""}).tickSize(0)) //tickFormat(()=>{return ""})to remove tick labels

    const yScale = d3.scaleLinear()
          .range([this.settings.height, 50]) //range goes in the opposite direction as compared to xScale because yScale grows from the bottom upwards
          .domain(this.key_dom(this.key_y))
    // Add y-axis
    // this.svgContainer.append("g")
    //   .attr("transform", "translate(" + this.settings.margin.left + "," + this.settings.margin.top + ")")
    //   .call(d3.axisLeft(yScale).tickFormat(()=>{return ""}).tickSize(0)) //tickFormat(()=>{return ""})to remove tick labels

    this.chartWrapper = this.svgContainer
          // .append("g")
          // .attr("class", "chartWrapper")
          .select("#chartWrapper")
          .attr("transform", "translate(" + this.settings.margin.left + "," + this.settings.margin.top + ")");
    
    // Add grid on chart
    // this.chartWrapper
    //   // .append("g")
    //   //   .attr("class", "grid")
    //     .select(".grid-x")
    //     .attr("transform", "translate(0," + this.settings.height + ")")
    //     .call(d3.axisBottom(xScale)
    //         .ticks(5)
    //         .tickSize(-this.settings.height)
    //         .tickFormat("")
    //     )
    // this.chartWrapper
    //     // .append("g")
    //     // .attr("class", "grid")
    //     .select(".grid-y")
    //     .call(d3.axisLeft(yScale)
    //         .ticks(5)
    //         .tickSize(-this.settings.width)
    //         .tickFormat("")
    //     )
    
    // Add axis labels   
    // this.chartWrapper.append("text")
    //       .attr("x", (this.settings.width / 2))
    //       .attr("y", this.settings.height + this.settings.margin.top)
    //       .attr("dy", "1em")
    //       .style("text-anchor", "middle")
    //       .style("fill", "#18A999")
    //       .text("PC2");

    // this.chartWrapper.append("text")
    //       .attr("transform", "rotate(-90)")
    //       .attr("y", 0 - this.settings.margin.left)
    //       .attr("x", 0 - (this.settings.height / 2))
    //       .attr("dy", "2.5em")
    //       .style("text-anchor", "middle")
    //       .style("fill", "#18A999")
    //       .text("PC1");

    const rScale = d3.scaleLinear()
			.range([15, 25])
      .domain(this.key_dom(this.key_r))

    // const colorScale = d3.scaleOrdinal(d3.schemeTableau10).domain(this.data_chart)

    // Use this if no annotation text
    // this.bubbles = 
    //   this.chartWrapper
    //   // .append('g')
    //   .select("#bubbles")
    //   .selectAll("dot")
    //   .data(this.data_chart)
    //   .enter()
    //   .append("circle")
    //     .attr("class", "bubble")
    //     .style("opacity", this.settings.opacityCircles)
    //     .style("fill", function(d) {return colorScale(d);})
    //     .style("stroke", "gray")
    //     .attr("cx", function(d) {return xScale(d.x);})
    //     .attr("cy", function(d) {return yScale(d.y);})
    //     .attr("r", function(d) {return rScale(d.count)})
       
    //Use this to add bubbles with annotation text 
    this.bubbles = this.chartWrapper
      .select("#bubbles")
      .selectAll("dot")
      .data(this.data_chart)
      .enter()
      .append("g")
      .attr("class", "bubble-container")
      .attr("id", d => `topic-${this.data_chart.indexOf(d)+1}`)

    this.bubbles.append("circle")
                .attr("class", "bubble")
                .attr("cx", function(d) {return xScale(d.x);})
                .attr("cy", function(d) {return yScale(d.y);})
                .attr("r", function(d) {return rScale(d.count)})
                .style("opacity", this.settings.opacityCircles)
                // .style("fill", function(d) {return colorScale(d);})
                .style("fill", "#bbc1be")
                .style("stroke", "white")
                .attr("stroke-width", 2)
                .on("click", (event, d) => this.onChange(event, this.data_chart.indexOf(d)+1))

    // Add bubble labels
    const topicNames = {
      1: "Friends & love", 
      2:"School activities", 
      3: "Fashion & shows", 
      4: "Anne's emotional world", 
      5: "Life at Green Gables", 
      6: "Incidents with the Barrys", 
      7: "Mistakes & apologies"
    }

    this.bubbles.append('text')
                .attr("class", "bubble-label")
                .attr("x", function(d) {return xScale(d.x)})
                .attr("y", function(d) {return d.count < 1200 ? yScale(d.y) - 22 
                                                              : d.count > 2000 ? yScale(d.y) - 30
                                                              : yScale(d.y) - 28}) //https://jonathansoma.com/lede/storytelling/d3/text-elements/ for difference between attr "x"/"y" and "dx"/"dy"
                .attr("text-anchor", "middle")
                .style("font-size", "12px")
                .text(d => topicNames[this.data_chart.indexOf(d)+1])
                .on("click", (event, d) => this.onChange(event, this.data_chart.indexOf(d)+1));

    // Add bubble tooltips
    const tooltip = d3.select('#bubble-chart').append("div")
                      .attr("id", "bubble-tooltip");
    
    /** @param {MouseEvent} e 
     * @param {Object} d //Each topic object in chart data
    */
    const handleMouseOver = (e, d) => {
      // const topTermsArray = termsData[this.data_chart.indexOf(d)].terms.slice(0, 10);
      // const topTerms = topTermsArray.map(i => i.name).join(", ")
      d3.select("#bubble-tooltip")
              .style("left", e.pageX + "px")
              .style("top", e.pageY + "px")
              .style("display", "block")
              .style("background", "white")
              .style("box-shadow", "3px 3px 10px rgba(0, 0, 0, 0.4)")
              .style("border-radius", "5px")
              .html("Frequency (in terms): " + d.count 
                    // + "<br>Top words: " + topTerms + ",..."
                    );
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
#bubbles {
  cursor: pointer; 
}

#bubble-tooltip {
    position: absolute;
    max-width: 200px;
    height: auto;
    padding: 4px;
    pointer-events: none;
    text-align: left;
    line-height: 16px;
    font-size: 12.5px;
  }
  
</style>