<template>
  <div id="app">
    <h2>Todo List</h2>
      <input type="text" v-model="item" v-on:keydown.enter="onKeyDown" />
      <button @click="additem">登録</button>
    <p>{{ $data }}</p>
  </div>
</template>

<script>
//以下のnameの内容がdivタグ内に適用される
export default {
  name: "app",
  //data型は関数じゃないとエラーが出る
  data() {
    return {
      item: "",
      todos: [],
    };
  },
  //処理内容
  methods: {
    onKeyDown() {
      // console.log(e.key)
      this.additem();
      },
    additem() {
      if (this.item == "") {
        alert("未入力")
        return;
      }
      this.todos.push(this.item);//配列名、Pushの中身は新しく入れる変数など
      this.item = "";
    },
  },
};
</script>

<style>
#app {
  text-align: center;
}

</style>
