<template>

    <div id="bubble-pack">
      <div id="main-chart">
        <BubbleChartMain
          :data="data"
          :data_chart="data_chart"
          :chapterKey="chapterKey"
        />
      </div>
      <BubbleChartLegend 
        :data="data"
        :data_chart="data_chart"
        :legend_class="legend_class" 
      />
    
    </div>
</template>

<script>
// import * as d3 from "d3";
import data from "@/data/bubblechart/topic_bubbles_data_7.json";
import BubbleChartMain from '@/components/BubbleChartMain.vue'
import BubbleChartLegend from '@/components/BubbleChartLegend.vue'

export default {
  props: {
    chapterKey: String
  },
  components: {
    BubbleChartMain,
    BubbleChartLegend
  },
  data() {
    return {
      data: [],
      data_chart: [],
      legend_class: [],
    }
  },
  created: function() {
    var that = this 
    that.data = Object.values(data)
    this.data_chart = this.data
  },
  // watch: {
  //   chapterKey: {
  //     deep: true,
  //     handler() {
  //       console.log(this.chapterKey)
  //       // const topTermsArray = termsData[this.data_chart.indexOf(d)].terms.slice(0, 10);
  //     }
  //   }
  // },
  methods: {
    
  }
}
</script>

<style>
  #bubble-pack {
    display: flex;
    flex-direction: row;
    margin: 0;
    padding: 0;
    height: 100%;
    width: 100%;
  }
</style>
