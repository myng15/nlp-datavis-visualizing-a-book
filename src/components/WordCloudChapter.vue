<template>
  <div id="wordcloud">
    <!-- <h3>Most frequent words per chapter</h3> -->
    <div><strong>Most frequent words:</strong> {{ selected }}
      <select style="font-family:'PT Sans', sans-serifSans" name="selected" @change="onChange($event)" v-model="key">
        <option disabled value="">Select a chapter</option>
        <option value="0">Entire Book</option>
        <option value="1">Chapter 1</option>
        <option value="2">Chapter 2</option>
        <option value="3">Chapter 3</option>
        <option value="4">Chapter 4</option>
        <option value="5">Chapter 5</option>
        <option value="6">Chapter 6</option>
        <option value="7">Chapter 7</option>
        <option value="8">Chapter 8</option>
        <option value="9">Chapter 9</option>
        <option value="10">Chapter 10</option>
        <option value="11">Chapter 11</option>
        <option value="12">Chapter 12</option>
        <option value="13">Chapter 13</option>
        <option value="14">Chapter 14</option>
        <option value="15">Chapter 15</option>
        <option value="16">Chapter 16</option>
        <option value="17">Chapter 17</option>
        <option value="18">Chapter 18</option>
        <option value="19">Chapter 19</option>
        <option value="20">Chapter 20</option>
        <option value="21">Chapter 21</option>
        <option value="22">Chapter 22</option>
        <option value="23">Chapter 23</option>
        <option value="24">Chapter 24</option>
        <option value="25">Chapter 25</option>
        <option value="26">Chapter 26</option>
        <option value="27">Chapter 27</option>
        <option value="28">Chapter 28</option>
        <option value="29">Chapter 29</option>
        <option value="30">Chapter 30</option>
        <option value="31">Chapter 31</option>
        <option value="32">Chapter 32</option>
        <option value="33">Chapter 33</option>
        <option value="34">Chapter 34</option>
        <option value="35">Chapter 35</option>
        <option value="36">Chapter 36</option>
        <option value="37">Chapter 37</option>
        <option value="38">Chapter 38</option>
         </select>
    </div>
  </div>
</template>

<script>

import * as d3 from "d3";
import data from "@/data/wordcloud/top_words_whole_book.json";
// eslint-disable-next-line no-unused-vars
import termsData from "@/data/bubblechart/top_terms_arr_per_topic_7.json";
import cloud from "d3-cloud"

export default {
  props: {
    topicKey: String
  },
  components: {},
  data() {
    return {key: ""};
  },
  watch: {
    key: {
      deep: true,
      handler() {
        d3.select("#wordcloud").select("svg").remove()
        this.init(this.key);
      }
    },
    topicKey: {
      deep: true,
      handler() {
        const topic = parseInt(this.topicKey);
        const chaptersOfTopic = termsData[topic-1].chapters;
        this.key = chaptersOfTopic[0];
        const otherChaptersOfTopic = chaptersOfTopic.slice(1).join(", ")
        d3.select("#wordcloud").select("#other-chapters").remove()
        d3.select("#wordcloud").select("svg").remove()
        d3.select("#wordcloud")
          .append("div")
          .attr("id", "other-chapters")
          .text(`Other chapters associated with this topic: ${otherChaptersOfTopic}`);
        this.init(this.key);
      }
    }
  },
  mounted() {
    this.init("0");
  },
  methods: {
    onChange() {
      this.$emit("changeChapter", this.key)
    },

    init(key) {
      var myWords = data[key];
// set the dimensions and margins of the graph
      var margin = {top: 2, right: 2, bottom: 2, left: 2},
          width = 400 - margin.left - margin.right,
          height = 400 - margin.top - margin.bottom;

      var wordSizes = myWords.map(d => d[1]);
      var colorScaleEntireBook = d3.scaleSequential()
                     .domain(d3.extent(wordSizes))
                     .interpolator(d3.interpolate("#9ca5a1", "#202321"));
      console.log(colorScaleEntireBook(122));
      var colorScaleChapters = d3.scaleSequential()
                     .domain(d3.extent(wordSizes))
                     .interpolator(d3.interpolate("#bbc1be", "#7d8983"));

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
          .padding(1)        //space between words
          .rotate(function () {
            return ~~(Math.random() * 2) * 90;
          }) // font size of words
          .font("Impact")
          .fontSize(function (d) {
            return key === "0" ? d.size/2 : d.size * 5;
          })
          .on("end", draw);
      layout.start();


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
              return key === "0" ? colorScaleEntireBook(d.size) : colorScaleChapters(d.size);
            }) //#96949E
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

h3 {
  margin-top: 0;

}

select {
  margin-bottom: 10px;
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