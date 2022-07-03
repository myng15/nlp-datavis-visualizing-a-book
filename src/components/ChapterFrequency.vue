<template>
  <div id="barchart"></div>
  <table id="objecttable">
    <thead>
    <tr><th>segment 1</th><th>segment 1</th><th>segment 3</th></tr>
    </thead>
    <tbody></tbody>
  </table>
</template>

<script>


/* eslint-disable */


import * as d3 from "d3";
import data from "@/data/barcharts/segmentInfo.json";

export default {
  name: "ChapterFrequency",
  mounted() {
    this.init();
  },
  methods: {
    init() {

      var matrix = [
        {name: "Lee Gai Fun", age: 42, sex: "M"},
        {name: "Laia Hamidullah", age: 27, sex: "F" },
        {name: "Abraham Mdulla", age: 33, sex: "M" }
      ];

      var tr = d3.select("#objecttable").select("tbody")
          .selectAll("tr")
          .data(matrix)
          .enter().append("tr");

      var td = tr.selectAll("td")
          .data(function(d, i) { return Object.values(d); })
          .enter().append("td")
          .text(function(d) { return d; });

    }}}

/* eslint-enable */
</script>


<style scoped>

</style>