<template>
  <div>
  <el-menu default-active="language" mode="horizontal" @select="handleSelect">
    <el-menu-item index="0" ><img src="../assets/github.jpeg" width="50" height="50" style="right: 0"/></el-menu-item>
    <el-menu-item index="language">语言排行</el-menu-item>
    <el-menu-item index="star">star比例</el-menu-item>
    <el-menu-item index="size">size比例</el-menu-item>
    <el-menu-item index="license">license比例</el-menu-item>
    <el-menu-item index="description">description比例</el-menu-item>
    <el-menu-item index="private">private比例</el-menu-item>
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
        <rank v-bind:year="year" v-bind:category="category" />
      </div>
      <div v-else style="margin-left: 50px; margin-top: 50px">
        <percent v-bind:year="year" v-bind:category="category" />
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
        category: "language"
      };
    },
    methods: {
      handleSelect(key) {
        if (key != 0) {
          this.category = key;
          console.log(this.category);
        }
        else{
          window.location.href = '/';
        }
      },
      handleYear(key) {
        this.year = key;
        console.log(this.year);
      },
    }
  }
</script>

