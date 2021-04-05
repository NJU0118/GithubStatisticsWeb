<template>
  <v-chart ref="line" class="chart" :option="option"/>
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
      category:String,
      keyData: {
        type: Array,
        // default: function () { return [] }
        default: () => []
      },
      valueData: {
        type: Array,
        // default: function () { return [] }
        default: () => []
      },
    },
    watch: {
      year (n, o) {
        this.option.legend.data = [n];
        this.option.series[0].name = n;
        this.option.title.subtext = n;
      },
      keyData(n, o){
        this.option.yAxis.data = n;
      },
      valueData(n, o){
        this.option.series[0].data = n;
      },
      category(n, o){
        this.option.title.text = this.categoryMapping(n);
      }
    },
    data() {
      return {
        option: {
          title: {
            text: this.categoryMapping(this.category),
            subtext: this.year
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
    },
    methods: {
      categoryMapping(category) {
        switch (category) {
          case 'language':
            return 'github语言排行';
          case 'star':
            return 'github项目Star数量分布';
          case 'size':
            return 'github项目大小分布';
        }
      }
    }
  };
</script>

<style scoped>
  .chart {
    height: 400px;
  }
</style>
