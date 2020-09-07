<template>
  <div id="app" class="app">
    <el-container>
      <HeaderBar></HeaderBar>
      <el-container class="is-vertical">
        <el-main>
          <router-view></router-view>
        </el-main>
        <BottomBar></BottomBar>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import HeaderBar from "./components/HeaderBar";
import BottomBar from "./components/BottomBar";

export default {
  components: {
    HeaderBar,
    BottomBar,
  },
  methods: {},
};
</script>

<style lang="less">
@import "assets/css/base.less";
</style>
