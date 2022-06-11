<template>
  <div id="wordcloud">
    <h3>Chapter 1</h3>
  </div>
</template>

<script>

import * as d3 from "d3";
import data from "@/data/top_words_by_chapter.json";
import cloud from "d3-cloud"

export default {
  components: {},
  mounted() {
    this.init();
  },
  data() {
    return {
      selectchapter: [
        {chapter: "1"},
        {chapter: "2"},
        {chapter: "3"},
        {chapter: "4"},
        {chapter: "5"}
      ]
    }
  },
  methods: {
    init() {

      var myWords = data["1"];

// set the dimensions and margins of the graph
      var margin = {top: 2, right: 2, bottom: 2, left: 2},
          width = 450 - margin.left - margin.right,
          height = 450 - margin.top - margin.bottom;

// append the svg object to the body of the page
      var svg = d3.select("#wordcloud").append("svg")
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
          .padding(4, 5)        //space between words
          .rotate(function () {
            return ~~(Math.random() * 2) * 90;
          }) // font size of words
          .fontSize(function (d) {
            return d.size * 7;
          })
          .on("end", draw);
      layout.start();

      function getRandomColor() {
        var letters = '0123456789ABCDEF';
        var color = '#';
        for (var i = 0; i < 6; i++) {
          color += letters[Math.floor(Math.random() * 16)];
        }
        return color;
      }

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
            .attr("fill", function () {
              return getRandomColor();
            })
            .attr("text-anchor", "middle")
            .style("font-family", "Impact")
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
  margin-top: 1em;
  position: relative;
}
</style>