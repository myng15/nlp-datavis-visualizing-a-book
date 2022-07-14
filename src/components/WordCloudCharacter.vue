<template>
  <div id="wordcloudcharacter">
  </div>
</template>

<script>

import * as d3 from "d3";
import data from "@/data/wordcloud/top_words_quotes_by_character.json";
import cloud from "d3-cloud"

export default {
  props: {
    characterKey: Object // {charName: String, charColor: String}
  },
  components: {},
  mounted() {
    // this.init();
  },
  /* eslint-disable */
  watch: {
    characterKey: {
      deep: true,
      handler() {
        d3.select("#wordcloudcharacter").select("svg").remove()
        this.init();
      }
    }
  },
  methods: {
    init() {
      var myWords = data[this.characterKey.charName]; 
      var characterColor = this.characterKey.charColor;
      var wordSizes = myWords.map(d => d[1]);
      var colorScale = d3.scaleSequential()
                     .domain(d3.extent(wordSizes))
                     .interpolator(d3.interpolate("#bbc1be", characterColor));
      
// set the dimensions and margins of the graph
      var margin = {top: 2, right: 2, bottom: 2, left: 2},
          width = 300 - margin.left - margin.right,
          height = 300 - margin.top - margin.bottom;


// append the svg object to the body of the page
      
      var svg = d3.select("#wordcloudcharacter").append("svg")
          .attr("width", width + margin.left + margin.right)
          .attr("height", height + margin.top + margin.bottom)
          .append("g")
          .attr("transform",
              "translate(" + margin.left + "," + margin.top + ")");

// Constructs a new cloud layout instance. It run an algorithm to find the position of words that suits your requirements
// Wordcloud features that are different from one word to the other must be here
      var layout = cloud()
          .size([width, height])
          .words(myWords.map(function (d) {
            return {text: d[0], size: d[1]};
          }))
          .padding(4,2)        //space between words
          .rotate(function () {
            return ~~(Math.random() * 2) * 90;
          }) // font size of words
          .fontSize(function (d) {
            return d3.min(wordSizes) >= 20 ? d.size : d3.min(wordSizes) >= 10 ? d.size*2 : d3.min(wordSizes) >= 3 ? d.size*4 : d.size*15;
          })
          .on("end", draw);
      layout.start();

      // If using random colors:
      function getRandomColor() {
        var letters = '0123456789ABCDEF';
        var color = '#';
        for (var i = 0; i < 6; i++) {
          color += letters[Math.floor(Math.random() * 16)];
        }
        return color;
      }

      // Use the same color as this character's node in node-link diagram
      // const characterColor = this.characterKey.charColor;
      // console.log(characterColor)
      
// This function takes the output of 'layout' above and draw the words
// Wordcloud features that are THE SAME from one word to the other can be here
      function draw(words) {
        svg
            .append("g")
            .attr("transform", "translate(" + layout.size()[0] / 2 + "," + layout.size()[1] / 2 + ")")
            .selectAll("text")
            .data(words)
            .enter().append("text")
            .style("font-size", function (d) {
              return d.size;
            })
            .attr("fill", function (d) {
              return colorScale(d.size);
            }
            )
            .attr("text-anchor", "middle")
            .style("font-family", "Impact")
            .transition()
            .duration(500)
            .attr("transform", function (d) {
              return "translate(" + [d.x, d.y] + ")rotate(" + d.rotate + ")";
            })
            .text(function (d) {
              return d.text;
            });
      }
    }
  }
};
/* eslint-enable */
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
  margin-top: 1em;
  position: relative;
}
</style>