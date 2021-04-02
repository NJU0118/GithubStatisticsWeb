<template>
  <v-chart class="chart" :option="option" />
</template>

<script>
  import VChart from "vue-echarts";
  export default {
    name: "rank",
    components: {
      VChart
    },
    props: {
      year:String,
      category:String
    },
    mounted() {
      console.log(this.year)
    },
    data() {
      return {
        option: {
          title: {
            text: 'github语言排行',
            subtext: '数据来自github'
          },
          tooltip: {
            trigger: 'axis',
            axisPointer: {
              type: 'shadow'
            }
          },
          legend: {
            data: [this.year]
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '3%',
            containLabel: true
          },
          xAxis: {
            type: 'value',
            boundaryGap: [0, 0.01]
          },
          yAxis: {
            type: 'category',
            data: ['java', 'c++', 'python', 'javascript', 'c', 'go']
          },
          series: [
            {
              name: this.year,
              type: 'bar',
              data: [18203, 23489, 29034, 104970, 131744, 630230],
              label: {
                show: true,
                position: 'right'
              },
            },
          ]
        }
      };
    }
  };
</script>

<style scoped>
  .chart {
    height: 400px;
  }
</style>
