<template>
  <div id="app">
    {{ condition }}
    <h2>Todo List</h2>
    <div class="weather">
      <P>大阪の天気は{{condition.main}}です。</P>
    </div>
    <div id="form">
      <input type="text" v-model="item" v-on:keydown.enter="onKeyDown" />
      <input type="date" v-model="date" v-on:keydown.enter="onKeyDown" />
      <button @click="additem">登録</button>
    </div>
    <div class="main">
      <table>
        <tr v-for="(todo, index) in todos" :key="`listitem${index}`">
          <td><input type="checkbox" v-model="todo.isDone" /></td>
          <td>{{ todo.newitem }}</td>
          <td>{{ todo.newdate | moment }}</td>
          <td>
            <button @click="change()">変更</button>
            <input type="text" v-show="view" v-model="todo.newitem" />
            <input type="date" v-show="view2" v-model="todo.newdate" />
          </td>
          <td>
            <button @click="deleteItem(index)">削除</button>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
moment.locale("ja");

//以下のnameの内容がdivタグ内に適用される
export default {
  name: "app",
  //data型は関数じゃないとエラーが出る
  data() {
    return {
      item: "",
      date: "",
      view: false,
      todos: [],
      city: "Osaka",
      temp: null,
      condition: {
        main: null,
      },
      apiUrl: "http://api.openweathermap.org/data/2.5/weather",
      apiKey: "1d6ea2750cd6e958153b8995b195109f",
    };
  },
  mounted() {
      axios
        .get(this.apiUrl, {
          params: { q: this.city, APPID: this.apiKey },
        })
        .then(
          function (response) {
            this.condition = response.data.weather[0];
          }.bind(this)
        )
        .catch(function (error) {
          alert(error);
        });
    },
  //処理内容
  methods: {
    onKeyDown() {
      this.additem();
    },
    additem() {
      if (this.item == "") {
        alert("未入力");
        return;
      }
      if (this.date == "") {
        this.date = new Date();
        this.date.getFullYear();
        this.date.getMonth();
        this.date.getDate();
        return;
      }
      var todo = {
        newitem: this.item,
        newdate: moment(this.date).format("YYYY-MM-DD"),
        // newdate:this.date
      };
      this.todos.push(todo);
      this.item = "";
    },
    deleteItem(index) {
      //indexを引数に指定
      this.todos.splice(index, 1); //indexで指定された要素を1つ削除
    },
    change() {
      this.view = !this.view;
      this.view2 = !this.view2;
    },

  },
};
</script>

<style>
#app h2 {
  text-align: center;
}

.weather {
  text-align: center;
}

#form {
  text-align: center;
}

.main {
  display: flex;
  justify-content: center;
}

.main li {
  text-align: left;
  list-style: none;
}
</style>
