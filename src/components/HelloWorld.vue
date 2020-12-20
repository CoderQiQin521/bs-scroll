<template>
  <div class="hello">
    <div class="wrapper" ref="wrapper">
      <ul class="content">
        <li class="item" v-for="item in 40" :key="item">{{ item }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import BetterScroll from "better-scroll";

export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  mounted() {
    // DOM渲染完成,正确计算高度,确保滚动正常
    this.$nextTick(() => {
      let bs = new BetterScroll(this.$refs.wrapper, {
        movable: true,
        zoom: true,
        pullUpLoad: true
      });
    });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  box-sizing: border-box;
}
ul {
  list-style-type: none;
  padding: 0;
}
.item {
  margin-bottom: 15px;
  border: 1px solid #eee;
  border-radius: 8px;
  padding: 5px 10px;
}
.item:last-child {
  margin-bottom: none;
}

.wrapper {
  height: 500px;
  overflow: hidden;
  border: 1px solid red;
}
</style>
