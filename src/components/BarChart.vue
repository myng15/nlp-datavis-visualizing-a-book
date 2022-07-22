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

import fakeChar from "@/data/barchart_sentiment_rel_sents/000_Chapterlegend.json";
import Anne from "@/data/barchart_sentiment_rel_sents/001_Anne Shirley.json";
import Alexander from "@/data/barchart_sentiment_rel_sents/015_Alexander Spencer.json";
import Diana from "@/data/barchart_sentiment_rel_sents/003_Diana Barry.json";
import Marilla from "@/data/barchart_sentiment_rel_sents/002_Marilla Cuthbert.json";
import Matthew from "@/data/barchart_sentiment_rel_sents/004_Matthew Cuthbert.json";
import Rachel from "@/data/barchart_sentiment_rel_sents/005_Rachel Lynde.json";
import Gilbert from "@/data/barchart_sentiment_rel_sents/006_Gilbert Blythe.json";
import Jane from "@/data/barchart_sentiment_rel_sents/007_Jane Andrews.json";
import Josie from "@/data/barchart_sentiment_rel_sents/008_Josie Pye.json";
import Josephine from "@/data/barchart_sentiment_rel_sents/014_Josephine Barry.json";
import Phillips from "@/data/barchart_sentiment_rel_sents/013_Mr. Phillips.json";
import Allan from "@/data/barchart_sentiment_rel_sents/010_Mrs. Allan.json";
import Barry from "@/data/barchart_sentiment_rel_sents/012_Mrs. Barry.json";
import Ruby from "@/data/barchart_sentiment_rel_sents/009_Ruby Gillis.json";
import Stacy from "@/data/barchart_sentiment_rel_sents/011_Miss Stacy.json";

export default {
  components: {BarChartLegend},
  mounted() {
    // NEW DATA
    // this.init(this.concatData(fakeChar), "Chapter");
    // this.init(this.concatData(Anne), "Anne Shirley");
    // this.init(this.concatData(Marilla), "Marilla Cuthbert");
    // this.init(this.concatData(Diana), "Diana Barry");
    // this.init(this.concatData(Matthew), "Matthew Cuthbert");
    // this.init(this.concatData(Rachel), "Rachel Lynde");
    // this.init(this.concatData(Gilbert), "Gilbert Blythe");
    // this.init(this.concatData(Jane), "Jane Andrews");
    // this.init(this.concatData(Josie), "Josie Pye");
    // this.init(this.concatData(Ruby), "Ruby Gillis");
    // this.init(this.concatData(Allan), "Mrs. Allan");
    // this.init(this.concatData(Stacy), "Miss Stacy");
    //  this.init(this.concatData(Barry), "Mrs. Barry");
    // this.init(this.concatData(Phillips), "Mr. Phillips");
    // this.init(this.concatData(Josephine), "Josephine Barry");
    // this.init(this.concatData(Alexander), "Alexander Spencer");    
    
    this.init(this.concatData(fakeChar), "Chapter");
    this.init(this.concatData(Rachel), "Rachel Lynde");
    this.init(this.concatData(Marilla), "Marilla Cuthbert");
    this.init(this.concatData(Matthew), "Matthew Cuthbert");
    this.init(this.concatData(Alexander), "Alexander Spencer");
    this.init(this.concatData(Diana), "Diana Barry");
    this.init(this.concatData(Jane), "Jane Andrews");
    this.init(this.concatData(Anne), "Anne Shirley");
    this.init(this.concatData(Barry), "Mrs. Barry");
    this.init(this.concatData(Ruby), "Ruby Gillis");
    this.init(this.concatData(Gilbert), "Gilbert Blythe");
    this.init(this.concatData(Phillips), "Mr. Phillips");
    this.init(this.concatData(Josie), "Josie Pye");
    this.init(this.concatData(Josephine), "Josephine Barry");
    this.init(this.concatData(Allan), "Mrs. Allan");
    this.init(this.concatData(Stacy), "Miss Stacy");
    
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
          .attr("style", name === "Chapter" ? "outline: none" :
                         "outline: solid 1px #bdbdbd")
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
          .style("fill",name === "Anne Shirley" ? "#548C2F" :
                        name === "Diana Barry" ? "#BC69AA" :
                        name === "Marilla Cuthbert" ? "#1C448E" :
                        name === "Matthew Cuthbert" ? "#ffa600" :
                        name === "Rachel Lynde" ? "#931F1D" :
                        "#313036")
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
          .attr("class", name === "Chapter" ? "" : "real-bar")
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

// Add barchart tooltips
    const tooltip = d3.select('#barchart-container').append("div")
                      .attr("id", "barchart-tooltip");
    
    /** @param {MouseEvent} e 
     * @param {Object} d //Each topic object in chart data
    */
    const handleMouseOver = (e, d) => {
      d3.select("#barchart-tooltip")
              .style("left", e.pageX + "px")
              .style("top", e.pageY + "px")
              .style("display", "block")
              .style("background", "white")
              .style("box-shadow", "3px 3px 10px rgba(0, 0, 0, 0.4)")
              .style("border-radius", "5px")
              .html(d.Segment + " (" + d.Chapter + "): <br>" +
                    + d.Mentions + " mentions");
      
    }
    
    d3.select('#barchart').selectAll(".real-bar").on("mouseover", handleMouseOver)
           .on("mouseout", () => {
              tooltip.style("display", "none");
    });

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
        // const domain = nrSegmentsPerChapter.map(chapter => {return x.bandwidth()*chapter})
        // const legendXScale = d3.scaleLinear()
        //                     .range([20, width])
        //                     .domain(d3.extent(domain))
        // svg.append('g').attr('transform', 'translate(0,' + height + ')')
        //                .call(d3.axisTop(legendXScale).tickValues(domain).tickFormat(function(d,i){ return legendChapters[i]}));

        //Add Chapter legend manually
        svg.selectAll("chapter-number")
        .data(data)
        .enter()
        .append("text")
        .attr("class", "chapter-number")
        .text(function(d) {
          return legendChapters.includes(d.Chapter.slice(8)) && isFirstSegment(d) ? d.Chapter.slice(8) : ""; //remove "Chapter ", take only chapter number
        })
          .attr("text-anchor", "end")
          .attr("fill", "black")
          .style("font-size", "12px")
          .attr("x", function(d) {
              return x(d.Segment) + x.bandwidth()*countSegmentsOfChapter(d) + x.bandwidth();
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

#barchart-tooltip {
    position: absolute;
    max-width: 90px;
    height: auto;
    margin: 0;
    padding: 2px 1px 2px 4px;
    pointer-events: none;
    line-height: 16px;
    text-align: left;
    font-size: 12.5px;
  }
</style>