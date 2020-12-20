<template>
  <div class="hello">
    <button @click="switchTab(1)">在职</button>
    <button @click="switchTab(2)">离职</button>
    <scroll
      class="wrapper"
      :data="list"
      :pullup="true"
      @scrollToEnd="scrollEnd"
    >
      <ul class="content">
        <li class="item" v-for="item in list" :key="item.id" @click="handle">
          {{ item.name }}
        </li>
      </ul>
      <div class="loading-wrapper">{{ loadingText }}</div>
    </scroll>
  </div>
</template>

<script>
import axios from "axios";
import scroll from "./better-scroll";

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
      pageInfo: {},
      loadingText: "玩命加载中"
    };
  },
  created() {
    this.loadData();
  },
  components: {
    scroll
  },
  mounted() {},
  methods: {
    scrollEnd() {
      console.log("this.pageInfo: ", this.pageInfo);
      console.log("this.page: ", this.page);
      // 接口还有数据的话,才继续请求
      if (this.page < this.pageInfo.last_page) {
        this.page++;
        this.loadData();
      } else {
        console.log("没有数据了");
      }
    },
    loadData(isSwitch) {
      let url = `http://192.168.2.159:3001/book/list?is_quit=${
        this.type === 1 ? 0 : 2
      }&page=${this.page}`;
      axios.get(url).then(res => {
        let data = res.data.data;
        this.pageInfo = data;
        // 切换重新复制     如果没有数据  不操作  判断
        if (this.page <= this.pageInfo.last_page) {
          this.list = isSwitch ? data.data : this.list.concat(data.data);
        }
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
