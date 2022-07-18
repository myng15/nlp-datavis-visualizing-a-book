<template>
  <div id="barchart-container">
    <h5> Character Occurrences and Sentiments</h5>
    <div id="barchart"></div>
    <BarChartLegend/>
  </div>
</template>

<script>
import * as d3 from "d3";
import BarChartLegend from "@/components/BarChartLegend"
// import Anne from "@/data/barchart_sentiment/Anne Shirley.json";
// import Alexander from "@/data/barchart_sentiment/Alexander Spencer.json";
// import Diana from "@/data/barchart_sentiment/Diana Barry.json";
// import Marilla from "@/data/barchart_sentiment/Marilla Cuthbert.json";
// import Matthew from "@/data/barchart_sentiment/Matthew Cuthbert.json";
// import Rachel from "@/data/barchart_sentiment/Rachel Lynde.json";
// import Gilbert from "@/data/barchart_sentiment/Gilbert Blythe.json";
// import Jane from "@/data/barchart_sentiment/Jane Andrews.json";
// import Josie from "@/data/barchart_sentiment/Josie Pye.json";
// import Josephine from "@/data/barchart_sentiment/Josephine Barry.json";
// import Phillips from "@/data/barchart_sentiment/Mr. Phillips.json";
// import Allan from "@/data/barchart_sentiment/Mrs. Allan.json";
// import Barry from "@/data/barchart_sentiment/Mrs. Barry.json";
// import Ruby from "@/data/barchart_sentiment/Ruby Gillis.json";
// import Stacy from "@/data/barchart_sentiment/Miss Stacy.json";

import fakeChar from "@/data/barchart_sentiment_rel_sents/Chapterlegend.json";
import Anne from "@/data/barchart_sentiment_rel_sents/Anne Shirley.json";
import Alexander from "@/data/barchart_sentiment_rel_sents/Alexander Spencer.json";
import Diana from "@/data/barchart_sentiment_rel_sents/Diana Barry.json";
import Marilla from "@/data/barchart_sentiment_rel_sents/Marilla Cuthbert.json";
import Matthew from "@/data/barchart_sentiment_rel_sents/Matthew Cuthbert.json";
import Rachel from "@/data/barchart_sentiment_rel_sents/Rachel Lynde.json";
import Gilbert from "@/data/barchart_sentiment_rel_sents/Gilbert Blythe.json";
import Jane from "@/data/barchart_sentiment_rel_sents/Jane Andrews.json";
import Josie from "@/data/barchart_sentiment_rel_sents/Josie Pye.json";
import Josephine from "@/data/barchart_sentiment_rel_sents/Josephine Barry.json";
import Phillips from "@/data/barchart_sentiment_rel_sents/Mr. Phillips.json";
import Allan from "@/data/barchart_sentiment_rel_sents/Mrs. Allan.json";
import Barry from "@/data/barchart_sentiment_rel_sents/Mrs. Barry.json";
import Ruby from "@/data/barchart_sentiment_rel_sents/Ruby Gillis.json";
import Stacy from "@/data/barchart_sentiment_rel_sents/Miss Stacy.json";

export default {
  components: {BarChartLegend},
  mounted() {
    // NEW DATA
    this.init(this.concatData(fakeChar), "Chapter");
    this.init(this.concatData(Anne), "Anne Shirley");
    this.init(this.concatData(Diana), "Diana Barry");
    this.init(this.concatData(Marilla), "Marilla Cuthbert");
    this.init(this.concatData(Matthew), "Matthew Cuthbert");
    this.init(this.concatData(Rachel), "Rachel Lynde");
    this.init(this.concatData(Gilbert), "Gilbert Blythe");
    this.init(this.concatData(Jane), "Jane Andrews");
    this.init(this.concatData(Josie), "Josie Pye");
    this.init(this.concatData(Phillips), "Mr. Phillips");
    this.init(this.concatData(Ruby), "Ruby Gillis");
    this.init(this.concatData(Alexander), "Alexander Spencer");
    this.init(this.concatData(Stacy), "Miss Stacy");
    this.init(this.concatData(Josephine), "Josephine Barry");
    this.init(this.concatData(Barry), "Mrs. Barry");
    this.init(this.concatData(Allan), "Mrs. Allan");
    
    // OLD DATA
    // this.init(Anne, "Anne Shirley");
    // this.init(Diana, "Diana Barry");
    // this.init(Marilla, "Marilla Cuthbert");
    // this.init(Matthew, "Matthew Cuthbert");
    // this.init(Rachel, "Rachel Lynde");
    // this.init(Gilbert, "Gilbert Blythe");
    // this.init(Jane, "Jane Andrews");
    // this.init(Josie, "Josie Pye");
    // this.init(Phillips, "Mr. Phillips");
    // this.init(Ruby, "Ruby Gillis");
    // this.init(Alexander, "Alexander Spencer");
    // this.init(Stacy, "Miss Stacy");
    // this.init(Josephine, "Josephine Barry");
    // this.init(Barry, "Mrs. Barry");
    // this.init(Allan, "Mrs. Allan");
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
          width = 550 - margin.left - margin.right,
          height = 20 - margin.top - margin.bottom;
      // append the svg object to the body of the page
      var svg = d3.select("#barchart")
          .append("div")
          .append("svg")
          .attr("id", name === "Chapter" ? "fake-char" : "")
          .attr("style", name === "Chapter" ? "outline: none" : "outline: solid 2px #bdbdbd")
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
          .attr("dy", name === "Chapter" ? "1.5em" : "1.1em")
          .attr("dx", "1em")
          .text(name);

// Bars
      svg.selectAll("mybar")
          .data(data)
          .enter()
          .append("rect")
          .attr("id", d => d.Chapter + "-" + d.Segment)
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


// Add chapter number tags on top
      const legendChapters = ["1", "6", "11", "16", "21", "26", "31", "36", "38"]
      const isFirstSegment = (d) => {
        const segmentsOfChapter = data.filter(item => item.Chapter === d.Chapter);
        const firstSegmentOfChapter = segmentsOfChapter[0] 
        return d.Segment === firstSegmentOfChapter.Segment;
      } 
      const countSegmentsOfChapter = (d) => {
        return data.filter(item => item.Chapter === d.Chapter).length;
      }
      const nrSegmentsPerChapter = []
      for (const chapter in fakeChar){
        nrSegmentsPerChapter.push(fakeChar[chapter].length)
      }
      
      if(name === "Chapter") {
        // Add Chapter legend using d3.axisTop
        // const domain = legendChapters.map(d => {return x(fakeChar[parseInt(d)].Segment) + x.bandwidth()*nrSegmentsPerChapter[0] - x.bandwidth()});
        const domain = nrSegmentsPerChapter.map(chapter => {return x.bandwidth()*chapter})
        const legendXScale = d3.scaleLinear()
                            .range([20, width])
                            .domain(d3.extent(domain))
        svg.append('g').attr('transform', 'translate(0,' + height + ')')
                       .call(d3.axisTop(legendXScale).tickValues(domain).tickFormat(function(d,i){ return legendChapters[i]}));

        //Add Chapter legend manually
        svg.selectAll("chapter-number")
        .data(data)
        .enter()
        .append("text")
        .attr("class", "chapter-number")
        .text(function(d) {
          return legendChapters.includes(d.Chapter.slice(8)) && isFirstSegment(d) ? d.Chapter.slice(8) : ""; //remove "Chapter ", take only chapter number
        })
          .attr("text-anchor", "middle")
          .attr("fill", "black")
          .style("font-size", "12px")
          .attr("x", function(d) {
              return x(d.Segment) + x.bandwidth()*countSegmentsOfChapter(d) - x.bandwidth();
          })
          .attr("y", function(d) {
              return height - y(d.Value) - 2;
          });
      }
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
/* .chapter-number {
  position: relative;
}
.chapter-number:before {
    content:""; 
    background: black; 
    position: absolute; 
    display: block;
    bottom: 0; 
    left: 0; 
    height: 1px; 
    width: 1.5px;
} */


</style>