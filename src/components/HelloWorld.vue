<template>
  <div class="hello">
    <div class="wrapper">
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
    let bs = new BetterScroll(".wrapper", {
      movable: true,
      zoom: true
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
