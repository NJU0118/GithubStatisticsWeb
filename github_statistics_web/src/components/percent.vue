<template>
  <v-chart ref="pie" class="chart" :option="option"/>
</template>

<script>
  import VChart from "vue-echarts";
  export default {
    name: "percent",
    components: {
      VChart
    },
    props: {
      year:String,
      category:String,
      pieData: {
        type: Array,
        // default: function () { return [] }
        default: () => []
      },
    },
    watch: {
      year (n, o) {
        this.option.title.subtext = n;
      },
      pieData(n, o){
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
            trigger: 'item'
          },
          legend: {
            top: '5%',
            left: 'center'
          },
          series: [
            {
              name: '项目数量',
              type: 'pie',
              radius: ['40%', '70%'],
              avoidLabelOverlap: false,
              itemStyle: {
                borderRadius: 10,
                borderColor: '#fff',
                borderWidth: 2
              },
              data: [
                {value: 1048, name: '有'},
                {value: 735, name: '无'}
              ]
            }
          ]
        }
      };
    },
    methods: {
      categoryMapping(category) {
        switch (category) {
          case 'license':
            return 'github项目有无license比例';
          case 'description':
            return 'github项目有无description比例';
          case 'private':
            return 'github项目是否为private比例';
          case 'fork':
            return 'github项目是否为fork比例';
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
