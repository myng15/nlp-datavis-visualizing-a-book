<template>
  <div id="barchart">
    <h5> Character Occurrences and Sentiments</h5>
  </div>
</template>

<script>
import * as d3 from "d3";
import Anne from "@/data/barchart_sentiment/Anne Shirley.json";
import Alexander from "@/data/barchart_sentiment/Alexander Spencer.json";
import Diana from "@/data/barchart_sentiment/Diana Barry.json";
import Marilla from "@/data/barchart_sentiment/Marilla Cuthbert.json";
import Matthew from "@/data/barchart_sentiment/Matthew Cuthbert.json";
import Rachel from "@/data/barchart_sentiment/Rachel Lynde.json";
import Gilbert from "@/data/barchart_sentiment/Gilbert Blythe.json";
import Jane from "@/data/barchart_sentiment/Jane Andrews.json";
import Josie from "@/data/barchart_sentiment/Josie Pye.json";
import Josephine from "@/data/barchart_sentiment/Josephine Barry.json";
import Phillips from "@/data/barchart_sentiment/Mr. Phillips.json";
import Allan from "@/data/barchart_sentiment/Mrs. Allan.json";
import Barry from "@/data/barchart_sentiment/Mrs. Barry.json";
import Ruby from "@/data/barchart_sentiment/Ruby Gillis.json";
import Stacy from "@/data/barchart_sentiment/Miss Stacy.json";

// import Anne from "@/data/barchart_sentiment_rel/Anne Shirley.json";
// import Alexander from "@/data/barchart_sentiment_rel/Alexander Spencer.json";
// import Diana from "@/data/barchart_sentiment_rel/Diana Barry.json";
// import Marilla from "@/data/barchart_sentiment_rel/Marilla Cuthbert.json";
// import Matthew from "@/data/barchart_sentiment_rel/Matthew Cuthbert.json";
// import Rachel from "@/data/barchart_sentiment_rel/Rachel Lynde.json";
// import Gilbert from "@/data/barchart_sentiment_rel/Gilbert Blythe.json";
// import Jane from "@/data/barchart_sentiment_rel/Jane Andrews.json";
// import Josie from "@/data/barchart_sentiment_rel/Josie Pye.json";
// import Josephine from "@/data/barchart_sentiment_rel/Josephine Barry.json";
// import Phillips from "@/data/barchart_sentiment_rel/Mr. Phillips.json";
// import Allan from "@/data/barchart_sentiment_rel/Mrs. Allan.json";
// import Barry from "@/data/barchart_sentiment_rel/Mrs. Barry.json";
// import Ruby from "@/data/barchart_sentiment_rel/Ruby Gillis.json";
// import Stacy from "@/data/barchart_sentiment_rel/Miss Stacy.json";

export default {
  components: {},
  mounted() {
    this.init(Anne, "Anne Shirley");
    this.init(Diana, "Diana Barry");
    this.init(Marilla, "Marilla Cuthbert");
    this.init(Matthew, "Matthew Cuthbert");
    this.init(Rachel, "Rachel Lynde");
    this.init(Gilbert, "Gilbert Blythe");
    this.init(Jane, "Jane Andrews");
    this.init(Josie, "Josie Pye");
    this.init(Phillips, "Mr. Phillips");
    this.init(Ruby, "Ruby Gillis");
    this.init(Alexander, "Alexander Spencer");
    this.init(Stacy, "Miss Stacy");
    this.init(Josephine, "Josephine Barry");
    this.init(Barry, "Mrs. Barry");
    this.init(Allan, "Mrs. Allan");
    // this.init(this.concatData(Anne), "Anne Shirley");
    // this.init(this.concatData(Diana), "Diana Barry");
    // this.init(this.concatData(Marilla), "Marilla Cuthbert");
    // this.init(this.concatData(Matthew), "Matthew Cuthbert");
    // this.init(this.concatData(Rachel), "Rachel Lynde");
    // this.init(this.concatData(Gilbert), "Gilbert Blythe");
    // this.init(this.concatData(Jane), "Jane Andrews");
    // this.init(this.concatData(Josie), "Josie Pye");
    // this.init(this.concatData(Phillips), "Mr. Phillips");
    // this.init(this.concatData(Ruby), "Ruby Gillis");
    // this.init(this.concatData(Alexander), "Alexander Spencer");
    // this.init(this.concatData(Stacy), "Miss Stacy");
    // this.init(this.concatData(Josephine), "Josephine Barry");
    // this.init(this.concatData(Barry), "Mrs. Barry");
    // this.init(this.concatData(Allan), "Mrs. Allan");
  },
  methods: {
    concatData(data){
      let newData = []
      for (const chapter in data){
        newData = newData.concat(data[chapter]);
      }
      return newData;
    },
    init(data, name) {
      // set the dimensions and margins of the graph
      var margin = {top:0 , right: 0, bottom: 0, left: 100},
          width = 620 - margin.left - margin.right,
          height = 20 - margin.top - margin.bottom;
// append the svg object to the body of the page
      var svg = d3.select("#barchart")
          .append("div")
          .append("svg")
          .attr("style", "outline: solid 2px #bdbdbd")
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
          .padding(0.4);
      svg.append("g")
//Getting max value to set length of y-axis
      var maxValue = Math.max(...data.map(o => o.Value))

// Add Y axis
      var y = d3.scaleLinear()
          .domain([0, maxValue])
          .range([height, 0]);
      svg.append("g")
      svg.append("text")
          .style("fill", "#313036")
          .style("font-size", "11px")
          .attr("class", "y label")
          .attr("text-anchor", "end")
          .attr("y", 1)
          .attr("dy", "0.9em")
          .attr("dx", "1em")
          .text(name);

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
          .attr("fill", function(d) {
            return d.Color;
          })

      // svg.selectAll("chapter-number")
      // .data(data)
      // .enter()
      // .append("text")
      // .attr("class", "chapter-number")
      // .text(function(d) {
      //   return "C"+d.Chapter.slice(8); //remove "Chapter ", take only chapter number
      // })
      //   .attr("text-anchor", "end")
      //   .attr("fill", "black")
      //   .style("font-size", "12px")
      //   // .attr("transform", "translate(" + 0 + "," + 20 + ")")
      //   .attr("x", function(d, i) {
      //       return i * (width / data.length) + 1;
      //   })
      //   .attr("y", function(d) {
      //       // return height - (d * 4);
      //       return height - y(d.Value) - 1;
      //   });

      // this.bubbles.append('text')
      //           .attr("class", "bubble-label")
      //           .attr("x", function(d) {return xScale(d.x)})
      //           .attr("y", function(d) {return d.count < 1200 ? yScale(d.y) - 22 
      //                                                         : d.count > 2000 ? yScale(d.y) - 30
      //                                                         : yScale(d.y) - 28}) //https://jonathansoma.com/lede/storytelling/d3/text-elements/ for difference between attr "x"/"y" and "dx"/"dy"
      //           .attr("text-anchor", "middle")
      //           .style("font-size", "12px")
      //           .text(d => topicNames[this.data_chart.indexOf(d)+1])
      //           .on("click", (event, d) => this.onChange(event, this.data_chart.indexOf(d)+1));

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
#barchart {
  margin-left: 20px;
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