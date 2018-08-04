<template>
  <div id="app">
    <h1 v-text="title"></h1>
    <input v-model="newItem" @keyup.enter="addNew">
    <ul>
      <li v-for="item in items" :key="item.id" :class="{finished:item.isFinished}" v-on:click="toggleFinish(item)">
        {{item.label}}
      </li>
    </ul>
  </div>
</template>

<script>
import Storage from "./storage.js";
export default {
  data() {
    return {
      title: "this is a todo list",
      items: Storage.fetch(), //数组
      newItem: ""
    };
  },
  //组件更新时调用
  watch: {
    items: {
      handler: function(items) {
        Storage.save(items);
      },
      deep: true
    }
  },
  methods: {
    //切换完成状态
    toggleFinish: function(item) {
      item.isFinished = !item.isFinished;
    },
    //添加新todo
    addNew: function() {
      this.items.push({
        id: this.items.length + 1,
        label: this.newItem,
        isFinished: false
      });
      this.newItem = "";
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.finished {
  text-decoration: underline;
}
</style>
