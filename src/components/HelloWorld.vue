<template>
  <div class="hello">
    <button @click="switchTab(1)">在职</button>
    <button @click="switchTab(2)">离职</button>
    <div class="wrapper" ref="wrapper">
      <ul class="content">
        <li class="item" v-for="item in list" :key="item.id" @click="handle">
          {{ item.name }}
        </li>
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
      list: [],
      type: 1,
      page: 1,
      pageInfo: {}
    };
  },
  created() {
    this.loadData();
  },
  mounted() {},
  methods: {
    loadData(isSwitch) {
      axios
        .get(
          `http://192.168.2.159:3001/book/list?is_quit=${
            this.type === 1 ? 0 : 2
          }&page=${this.page}`
        )
        .then(res => {
          let data = res.data.data;
          this.pageInfo = data;
          // 切换重新复制     如果没有数据  不操作  判断
          if (data.data.length) {
            this.list = isSwitch ? data.data : this.list.concat(data.data);
          }
          // console.log("data.data: ", data.data);
          // DOM渲染完成,正确计算高度,确保滚动正常
          // !Vue 数据发生变化（this.data = res.data）到页面重新渲染是一个异步的过程，我们的初始化时机是要在 DOM 重新渲染后，所以这里用到了 this.$nextTick，当然替换成 setTimeout(fn, 20) 也是可以的。
          this.$nextTick(() => {
            if (!this.scroll) {
              this.scroll = new BetterScroll(this.$refs.wrapper, {
                click: true,
                pullUpLoad: true // 上拉加载
                // probeType: 3
              });
              this.scroll.on("touchEnd", pos => {
                if (pos > 50) {
                  console.log("touchEnd");
                  alert("touchEnd");
                  this.loadData();
                }
              });
              this.scroll.on("pullingUp", () => {
                console.log("pullingUp");
                // 接口还有数据的话,才继续请求
                if (this.page < this.pageInfo.last_page) {
                  this.page++;
                  this.loadData();
                } else {
                  console.log("没有数据了");
                }
                this.scroll.finishPullUp();
              });
            } else {
              this.scroll.refresh();
            }
          });
        });
    },
    switchTab(type) {
      if (this.type === type) {
        return;
      }
      this.type = type;
      this.page = 1;
      console.log("switchTab: ", {
        type: this.type,
        page: this.page
      });
      this.loadData(true);
    },
    handle() {
      alert(1);
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
