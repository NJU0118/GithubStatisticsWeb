<template>
  <div>
  <el-menu :default-active="activeIndex" mode="horizontal" @select="handleSelect">
    <el-menu-item index="0" ><img src="../assets/github.jpeg" width="50" height="50" style="right: 0"/></el-menu-item>
    <el-menu-item index="1">语言排行</el-menu-item>
    <el-menu-item index="2">star比例</el-menu-item>
    <el-menu-item index="3">size比例</el-menu-item>
    <el-menu-item index="4">license比例</el-menu-item>
    <el-menu-item index="5">description比例</el-menu-item>
    <el-menu-item index="6">private比例</el-menu-item>
    <el-menu-item index="7">fork比例</el-menu-item>
  </el-menu>
    <el-col :span="3">
      <h5>选择年份</h5>
      <el-menu
        default-active="1"
        class="el-menu-vertical-demo"
        @open="handleOpen"
        @close="handleClose">
        <el-menu-item index="1">
          <i class="el-icon-menu"></i>
          <span slot="title">总览</span>
        </el-menu-item>
        <el-menu-item index="2">
          <i class="el-icon-document"></i>
          <span slot="title">2016</span>
        </el-menu-item>
        <el-menu-item index="3">
          <i class="el-icon-document"></i>
          <span slot="title">2017</span>
        </el-menu-item>
        <el-menu-item index="4">
          <i class="el-icon-document"></i>
          <span slot="title">2018</span>
        </el-menu-item>
        <el-menu-item index="5">
          <i class="el-icon-document"></i>
          <span slot="title">2019</span>
        </el-menu-item>
        <el-menu-item index="6">
          <i class="el-icon-document"></i>
          <span slot="title">2020</span>
        </el-menu-item>
      </el-menu>
    </el-col>
  </div>
</template>

<script>
    export default {
        name: "MainPage",
      data() {
        return {
          activeIndex: '1',
          activeIndex2: '1'
        };
      },
      methods: {
        handleSelect(key, keyPath) {
          console.log(key, keyPath);
        }
      }
    }
</script>

