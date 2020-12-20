<template>
  <div class="hello">
    <button @click="loadData(1)">分类1</button>
    <button @click="loadData(2)">分类2</button>
    <div class="wrapper" ref="wrapper">
      <ul class="content">
        <li class="item" v-for="item in list" :key="item">{{ item }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import BetterScroll from "better-scroll";

export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      list: []
    };
  },
  created() {
    this.loadData();
  },
  mounted() {},
  methods: {
    loadData(type = 1) {
      axios.get("http://localhost:3001/book/list?type=" + type).then(res => {
        let data = res.data;
        this.list = data.data;
        console.log("data.data: ", data.data);
        // DOM渲染完成,正确计算高度,确保滚动正常
        // !Vue 数据发生变化（this.data = res.data）到页面重新渲染是一个异步的过程，我们的初始化时机是要在 DOM 重新渲染后，所以这里用到了 this.$nextTick，当然替换成 setTimeout(fn, 20) 也是可以的。
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BetterScroll(this.$refs.wrapper, {});
          } else {
            this.scroll.refresh();
          }
        });
      });
    }
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
