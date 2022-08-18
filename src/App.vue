<template>
  <div id="app">
    <h2>Todo List</h2>
      <div id="form">
        <input type="text" v-model="item" v-on:keydown.enter="onKeyDown" />
        <button @click="additem">登録</button>
      </div>
    <div class="main">
      <ul>
        <li v-for="(todo, index) in todos" :key="`listitem${index}`">
          <input type="checkbox" v-model="todo.isDone">
          <p>{{todo.newitem}}</p>
          <button @click="change()">変更</button>
            <input type="text" v-show="view" v-model="todo.newitem">
          <button @click="deleteItem(index)">削除</button>
        </li> 
      </ul>
    </div>
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
      view:false,
      todos: [],

    };
  },
  //処理内容
  methods: {
    onKeyDown() {
      this.additem();
    },
    additem() {
      if (this.item == "") {
        alert("未入力")
        return;
      }
      var todo = {
        newitem: this.item
      };
      this.todos.push(todo);
      this.item = "";
    },
    deleteItem(index){ //indexを引数に指定
    this.todos.splice(index,1) //indexで指定された要素を1つ削除
    },
    change(){
      this.view = !this.view;
    },
  },

  // name:"change",
  // methods: {

  // }
};
</script>

<style>
#app h2{
  text-align: center;
}

#form{
  text-align: center;
}

.main {
  display: flex;
  justify-content: center;
}

.main li{
  text-align:left;
  list-style: none;
}
</style>
