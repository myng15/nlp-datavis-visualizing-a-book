<template>
<div id="barchartlegend"></div>
</template>

<script>
import * as d3 from "d3";
export default {
  name: "BarChartLegend",
  mounted() {
    this.init();

  },
  methods: {
    init() {
      const blue = "#8db6dc" //negative 
      const gray = "#7f8285" //neutral 
      const red = "#f995a6" //positive
      
      function Legend(color, {
        title,
        tickSize = 6,
        width = 320, 
        height = 46 + tickSize,
        marginTop = 18,
        marginRight = 0,
        marginBottom = 16 + tickSize,
        marginLeft = 0,
        ticks = 2,
        tickFormat,
        tickValues
      } = {}) {

        function ramp(color, n = 256) {
          const canvas = document.createElement("canvas");
          canvas.width = n;
          canvas.height = 1;
          const context = canvas.getContext("2d");
          for (let i = 0; i < n; ++i) {
            context.fillStyle = color(i / (n - 1));
            context.fillRect(i, 0, 1, 1);
          }
          return canvas;
        }

        const svg = d3.select("#barchartlegend")
              .append("svg")
              .attr("width", width)
              .attr("height", height)
              .attr("transform", "translate(" + (width - width/2 - 20) + ",0)")
              .attr("viewBox", [0, 0, width, height])
              .style("overflow", "visible")
              .style("display", "block");

        let tickAdjust = g => g.selectAll(".tick line").attr("y1", marginTop + marginBottom - height);
        let x;

        
        const n = Math.min(color.domain().length, color.range().length);

        x = color.copy().rangeRound(d3.quantize(d3.interpolate(marginLeft, width - marginRight), n));

        svg.append("image")
            .attr("x", marginLeft)
            .attr("y", marginTop)
            .attr("width", width - marginLeft - marginRight)
            .attr("height", height - marginTop - marginBottom)
            .attr("preserveAspectRatio", "none")
            .attr("xlink:href", ramp(color.copy().domain(d3.quantize(d3.interpolate(0, 1), n))).toDataURL());
      

        svg.append("g")
            .attr("transform", `translate(0,${height - marginBottom})`)
            .call(d3.axisBottom(x)
              .ticks(ticks, typeof tickFormat === "string" ? tickFormat : undefined)
              .tickFormat(typeof tickFormat === "function" ? tickFormat : undefined)
              .tickSize(tickSize)
              .tickValues(tickValues))
            .call(tickAdjust)
            .call(g => g.select(".domain").remove())
            .call(g => g.append("text")
              .attr("x", marginLeft)
              .attr("y", marginTop + marginBottom - height - 6)
              .attr("fill", "currentColor")
              .attr("text-anchor", "start")
              .attr("font-weight", "bold")
              .style("font-size", "11px")
              .attr("class", "title")
              .text(title))
            // .call(g => g.append("text")
            //   .attr("x", marginLeft - 50)
            //   .attr("y", 0)
            //   .attr("fill", "currentColor")
            //   .attr("text-anchor", "start")
            //   .attr("font-weight", "bold")
            //   .attr("class", "sentiment-tag")
            //   .text("Negative"))
            // .call(g => g.append("text")
            //   .attr("x", width + marginRight + 10)
            //   .attr("y", 0)
            //   .attr("fill", "currentColor")
            //   .attr("text-anchor", "start")
            //   .attr("font-weight", "bold")
            //   .attr("class", "sentiment-tag")
            //   .text("Positive"))
            ;

        return svg.node();
      }
     
      // Draw color legend
      Legend(d3.scaleDiverging([-1, 0, 1], [blue, gray, red]), {
        title: "Sentiment",
        tickFormat: (d,i) => {return ["Negative", "Neutral", "Positive"][i]}
      })
      

      //Alternative color legend
    //   const width = 250;
    //   const height = 15;
    //   const margin = {top: 10, bottom: 0, left: 0, right: 0};
    //   const svg = d3.select("#barchartlegend")
    //     .append("svg")
    //     .attr("width", (width))
    //     .attr("height", (height))
    //     // .attr("transform", "rotate(90)")
    //     .style("margin", margin.top);
      
    //   const legendWrapper = svg.append("g").attr("id", "legendWrapper").attr("width", width)
    //     .attr("height", height)
    //   const linearGradient = legendWrapper.append("linearGradient")
    //   .attr("id", "linear-gradient");

    //   linearGradient.append("stop")
    //       .attr("offset", "0%")
    //       .attr("stop-color", blue); 

    //   linearGradient.append("stop")
    //       .attr("offset", "50%")
    //       .attr("stop-color", gray); 

    //   linearGradient.append("stop")
    //       .attr("offset", "100%")
    //       .attr("stop-color", red); 

    //   legendWrapper.append("rect")
    //       .attr("width", width)
    //       .attr("height", height)
    //       .style("fill", "url(#linear-gradient)");
      
    //   // legendWrapper.append("svg").attr("id", "color-legend")
    //   // const valuesToShow = ["Negative", "Neutral", "Positive"]
    //   var x = d3.scaleLinear()
    //     .domain([-1, 0, 1])
    //     .range([0, width]);

    // var axis = d3.axisBottom(x);

    // legendWrapper
    //     .append("g")
    //     .attr("id", "g-runoff")
    //     .attr("class", "axis")
    //     .attr("width", width)
    //     .attr("height", height)
    //     .attr("transform", "translate(0,20)")
    //     .attr("fill", "black")
    //     .call(axis.ticks(2));



// Alina's code
//       var colorScale1 = d3.scaleSequential(d3.interpolatePlasma)
//           .domain([0, 20]);
//       var colorScale2 = d3.scaleSequential(d3.interpolateViridis)
//           .domain([0, 1000]);
//       var colorScale3 = d3.scaleSequential(d3.interpolateRainbow)
//           .domain([0, 90]);
//       var colorScale4 = d3.scaleSequential(d3.interpolateGnBu)
//           .domain([0, 0.2]);
//       var colorScale5 = d3.scaleSequential(d3.interpolatePRGn)
//           .domain([-5, 5]);

//       continuous("#legend1", colorScale1);
//       continuous("#legend2", colorScale2);
//       continuous("#legend3", colorScale3);
//       continuous("#legend4", colorScale4);
//       continuous("#legend5", colorScale5);



// // create continuous color legend
//       function continuous(selector_id, colorscale) {
//         var legendheight = 200,
//             legendwidth = 80,
//             margin = {top: 10, right: 60, bottom: 10, left: 2};

//         var canvas = d3.select(selector_id)
//             .style("height", legendheight + "px")
//             .style("width", legendwidth + "px")
//             .style("position", "relative")
//             .append("canvas")
//             .attr("height", legendheight - margin.top - margin.bottom)
//             .attr("width", 1)
//             .style("height", (legendheight - margin.top - margin.bottom) + "px")
//             .style("width", (legendwidth - margin.left - margin.right) + "px")
//             .style("border", "1px solid #000")
//             .style("position", "absolute")
//             .style("top", (margin.top) + "px")
//             .style("left", (margin.left) + "px")
//             .node();

//         var ctx = canvas.getContext("2d");

//         var legendscale = d3.scaleLinear()
//             .range([1, legendheight - margin.top - margin.bottom])
//             .domain(colorscale.domain());

//         // image data hackery based on http://bl.ocks.org/mbostock/048d21cf747371b11884f75ad896e5a5
//         var image = ctx.createImageData(1, legendheight);
//         d3.range(legendheight).forEach(function(i) {
//           var c = d3.rgb(colorscale(legendscale.invert(i)));
//           image.data[4*i] = c.r;
//           image.data[4*i + 1] = c.g;
//           image.data[4*i + 2] = c.b;
//           image.data[4*i + 3] = 255;
//         });
//         ctx.putImageData(image, 0, 0);

//         // A simpler way to do the above, but possibly slower. keep in mind the legend width is stretched because the width attr of the canvas is 1
//         // See http://stackoverflow.com/questions/4899799/whats-the-best-way-to-set-a-single-pixel-in-an-html5-canvas
//         /*
//         d3.range(legendheight).forEach(function(i) {
//           ctx.fillStyle = colorscale(legendscale.invert(i));
//           ctx.fillRect(0,i,1,1);
//         });
//         */

//         var legendaxis = d3.axisRight()
//             .scale(legendscale)
//             .tickSize(6)
//             .ticks(8);

//         var svg = d3.select(selector_id)
//             .append("svg")
//             .attr("height", (legendheight) + "px")
//             .attr("width", (legendwidth) + "px")
//             .style("position", "absolute")
//             .style("left", "0px")
//             .style("top", "0px")

//         svg
//             .append("g")
//             .attr("class", "axis")
//             .attr("transform", "translate(" + (legendwidth - margin.left - margin.right + 3) + "," + (margin.top) + ")")
//             .call(legendaxis);
//       }
    }
  }
}
</script>

<style scoped>
</style>