<template>
  <div>
  <el-menu default-active="language" mode="horizontal" @select="handleSelect">
    <el-menu-item index="0" ><img src="../assets/github.jpeg" width="50" height="50" style="right: 0"/></el-menu-item>
    <el-menu-item index="language">语言排行</el-menu-item>
    <el-menu-item index="star">star分布</el-menu-item>
    <el-menu-item index="size">大小分布</el-menu-item>
    <el-menu-item index="license">license比例</el-menu-item>
    <el-menu-item index="description">description比例</el-menu-item>
    <el-menu-item index="fork">fork比例</el-menu-item>
  </el-menu>
    <el-col :span="3">
      <h5>选择年份</h5>
      <el-menu
        default-active="总览"
        class="el-menu-vertical-demo"
        @select="handleYear">
        <el-menu-item index="总览">
          <i class="el-icon-menu"></i>
          <span slot="title">总览</span>
        </el-menu-item>
        <el-menu-item index="2016">
          <i class="el-icon-document"></i>
          <span slot="title">2016</span>
        </el-menu-item>
        <el-menu-item index="2017">
          <i class="el-icon-document"></i>
          <span slot="title">2017</span>
        </el-menu-item>
        <el-menu-item index="2018">
          <i class="el-icon-document"></i>
          <span slot="title">2018</span>
        </el-menu-item>
        <el-menu-item index="2019">
          <i class="el-icon-document"></i>
          <span slot="title">2019</span>
        </el-menu-item>
        <el-menu-item index="2020">
          <i class="el-icon-document"></i>
          <span slot="title">2020</span>
        </el-menu-item>
      </el-menu>
    </el-col>
    <el-col :span="12">
      <div v-if="category === 'language' || category === 'size' || category === 'star'" style="margin-left: 50px; margin-top: 50px">
        <rank v-bind:year="year" v-bind:category="category" :keyData="keyData" :valueData="valueData"/>
      </div>
      <div v-else style="margin-left: 50px; margin-top: 50px">
        <percent v-bind:year="year" v-bind:category="category" :pieData="pieData"/>
      </div>
    </el-col>
  </div>
</template>

<script>
  import rank from './rank'
  import Percent from "./percent";
  export default {
    name: "MainPage",
    components:{
      Percent,
        rank
    },
    data() {
      return {
        activeIndex: 'language',
        year: "总览",
        category: "language",
        //key和value一一对应，是柱状图的，从小到大；pieData是饼图的；这个初始化是实例。
        keyData: [],
        valueData: [],
        pieData:[]
      };
    },
    mounted() {
      this.$axios
        .get('/server/language/count?year='+this.year)
        .then(re=>{
          let rows = [];
          rows = re.data.rows;
          for(let i=0; i<rows.length; i++){
            this.keyData.push(rows[i].language);
            this.valueData.push(rows[i].count);
          }
        })
    },
    methods: {
      //变化函数，可以在这里加axios
      handleSelect(key) {
        if (key != 0) {
          this.category = key;
          switch (this.category) {
            case "language":
              this.keyData.splice(0, this.keyData.length);
              this.valueData.splice(0, this.valueData.length);
              this.$axios
                .get('/server/language/count?year='+this.year)
                .then(re=>{
                  let rows = [];
                  rows = re.data.rows;
                  for(let i=0; i<rows.length; i++){
                    this.keyData.push(rows[i].language);
                    this.valueData.push(rows[i].count);
                  }
                  this.keyData.reverse();
                  this.valueData.reverse();
                  console.log(this.keyData);
                  console.log(this.valueData);
                });
              break;
            case "star":
              this.keyData.splice(0, this.keyData.length);
              this.valueData.splice(0, this.valueData.length);
              this.$axios
                .get('/server/star/count?year='+this.year)
                .then(re=>{
                  let star_count = re.data;
                  this.keyData.push("0-10");
                  this.valueData.push(star_count.level0Count);
                  this.keyData.push("11-100");
                  this.valueData.push(star_count.level1Count);
                  this.keyData.push("101-1000");
                  this.valueData.push(star_count.level2Count);
                  this.keyData.push("1001-∞");
                  this.valueData.push(star_count.level3Count);
                  this.keyData.reverse();
                  this.valueData.reverse();
                });
              break;
            case "size":
              this.keyData.splice(0, this.keyData.length);
              this.valueData.splice(0, this.valueData.length);
              this.$axios
                .get('/server/size/count?year='+this.year)
                .then(re=>{
                  let star_count = re.data;
                  this.keyData.push("0-100");
                  this.valueData.push(star_count.level0Count);
                  this.keyData.push("101-1000");
                  this.valueData.push(star_count.level1Count);
                  this.keyData.push("1001-10000");
                  this.valueData.push(star_count.level2Count);
                  this.keyData.push("10001-∞");
                  this.valueData.push(star_count.level3Count);
                  this.keyData.reverse();
                  this.valueData.reverse();
                });
              break;
            case "license":
              this.pieData.splice(0, this.pieData.length);
              this.$axios
                .get('/server/license/count?year='+this.year)
                .then(re=>{
                  let license = re.data;
                  this.pieData.push({value:license.hasLicenseCount, name:"有"});
                  this.pieData.push({value:license.noLicenseCount, name:"无"});
                });
              break;
            case "description":
              this.pieData.splice(0, this.pieData.length);
              this.$axios
                .get('/server/description/count?year='+this.year)
                .then(re=>{
                  let description = re.data;
                  this.pieData.push({value:description.hasDescriptionCount, name:"有"});
                  this.pieData.push({value:description.noDescriptionCount, name:"无"});
                });
              break;
          }
        }
        else{
          window.location.href = '/';
        }
      },
      handleYear(key) {
        this.year = key;
        switch (this.category) {
          case "language":
            this.keyData.splice(0, this.keyData.length);
            this.valueData.splice(0, this.valueData.length);
            this.$axios
              .get('/server/language/count?year='+this.year)
              .then(re=>{
                let rows = [];
                rows = re.data.rows;
                for(let i=0; i<rows.length; i++){
                  this.keyData.push(rows[i].language);
                  this.valueData.push(rows[i].count);
                }
                this.keyData.reverse();
                this.valueData.reverse();
                console.log(this.keyData);
                console.log(this.valueData);
              });
            break;
          case "star":
            this.keyData.splice(0, this.keyData.length);
            this.valueData.splice(0, this.valueData.length);
            this.$axios
              .get('/server/star/count?year='+this.year)
              .then(re=>{
                let star_count = re.data;
                this.keyData.push("0-10");
                this.valueData.push(star_count.level0Count);
                this.keyData.push("11-100");
                this.valueData.push(star_count.level1Count);
                this.keyData.push("101-1000");
                this.valueData.push(star_count.level2Count);
                this.keyData.push("1001-∞");
                this.valueData.push(star_count.level3Count);
                this.keyData.reverse();
                this.valueData.reverse();
              });
            break;
          case "size":
            this.keyData.splice(0, this.keyData.length);
            this.valueData.splice(0, this.valueData.length);
            this.$axios
              .get('/server/size/count?year='+this.year)
              .then(re=>{
                let star_count = re.data;
                this.keyData.push("0-100");
                this.valueData.push(star_count.level0Count);
                this.keyData.push("101-1000");
                this.valueData.push(star_count.level1Count);
                this.keyData.push("1001-10000");
                this.valueData.push(star_count.level2Count);
                this.keyData.push("10001-∞");
                this.valueData.push(star_count.level3Count);
                this.keyData.reverse();
                this.valueData.reverse();
              });
            break;
          case "license":
            this.pieData.splice(0, this.pieData.length);
            this.$axios
              .get('/server/license/count?year='+this.year)
              .then(re=>{
                let license = re.data;
                this.pieData.push({value:license.hasLicenseCount, name:"有"});
                this.pieData.push({value:license.noLicenseCount, name:"无"});
              });
            break;
          case "description":
            this.pieData.splice(0, this.pieData.length);
            this.$axios
              .get('/server/description/count?year='+this.year)
              .then(re=>{
                let description = re.data;
                this.pieData.push({value:description.hasDescriptionCount, name:"有"});
                this.pieData.push({value:description.noDescriptionCount, name:"无"});
              });
            break;
        }
      },
    }
  }
</script>

