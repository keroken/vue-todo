<template>
  <div id="app">
    <div class="input__container">
        <h1>VueTodo</h1>
        <input class="input__text" type="text" v-model="text" v-on:keyup.enter="createNewToDoItem" />
        <button class="input__btn" @click="createNewToDoItem()"><i class="ion-ios-plus-outline"></i></button>
    </div>
    <ul class="list__container">
        <TodoItem class="list__container" v-for="item in list" :item="item" @delete="deleteItem" @toggle="toggleItem" :key="item.id" />
      </ul>
  </div>
</template>

<script>
import TodoItem from './components/TodoItem'

export default {
  name: 'app',
  components: {
    TodoItem
  },
  data: () => {
    return {
      list: [],
      text: '',
    };
  },
  methods: {
    createNewToDoItem() {
      //validate todo
      if (!this.text) {
        alert("Please enter a todo!");
        return;
      }
      this.list.push({ text: this.text, key: Date.now(), done: false });
      todoStorage.save(this.list);
      this.text = '';
    },
    deleteItem(key) {
      this.list = this.list.filter(item => item.key !== key);
      todoStorage.save(this.list);
    },
    toggleItem(key) {
      const itemIndex = this.list.findIndex(item => { return item.key == key});
      this.list[itemIndex].done = !this.list[itemIndex].done;
      todoStorage.save(this.list);
    },
  },
  created() {
    this.list = todoStorage.fetch();
  }
}

const STORAGE_KEY = 'todos-vuejs'
const todoStorage = {
  fetch: function() {
    var list = JSON.parse(
      localStorage.getItem(STORAGE_KEY) || '[]'
    )
    return list
  },
  save: function(todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
* {
  margin: 0;
  padding: 0;
}

html {
  box-sizing: border-box;
}

body {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  font-size: 18px;
  font-weight: 900;
  color: #2c3e50;
  height: 100vh;
}

.btn {
  font-size: 18px;
}

.App {
  text-align: center;
}

.app-title {
  font-size: 28px;
  color: deepskyblue;
}

.input__container {
  height: 80px;
  padding: 20px 10px;
  background-color: greenyellow;
}

.input__text {
  margin-top: 7px;
  outline: none;
  border: none;
  border-radius: 3px;
  padding: 5px 7px;
  margin: 7px;
}

.input__btn {
  font-size: 18px;
  display: inline-block;
  margin-left: 10px;
  background-color: greenyellow;
  border: none;
  transition: all .2s;
}

.input__btn:hover {
  transform: scale(1.2);
}

.input__btn:focus {
  outline: none;
}

.list__container {
  background-color: rgb(38, 228, 221);
  list-style: none;
}

</style>
