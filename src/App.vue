<template>
  <div id="app">
    <div class="container">
      <h1 v-text="text"></h1>
      <input type="text" v-model="newItem" v-on:keyup.enter="addNew" />
      <button v-on:click="addNew">Add</button>
      <ul>
        <li v-for="item in items" v-bind:class="{finished: item.isFinished}" v-on:click="toggle(item)">{{item.label}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
// import Hello from './components/Hello'
import Store from './store';

export default {
  name: 'app',
  data() {
    return {
      text: 'Todo List',
      items: Store.fetch(),
      newItem: ''
    }
  },
  watch: {
    items: {
      handler(items) {
        Store.save(items);
      },
      deep: true
    }
  },
  methods: {
    toggle(item) {
      item.isFinished = !item.isFinished;
    },
    addNew() {
      if (this.newItem.trim() == '') {
        return;
      }
      this.items.push({label: this.newItem, isFinished: false});
      this.newItem = '';
    }
  },
  components: {}
}
</script>

<style>
.container {
  text-align: center;
}
input {
  box-shadow: 0 0 8px rgb(150, 104, 219);
  border: none;
}
button {
  border-radius: 14px;
  background-color: rgba(0, 0, 0, .1);
  color: black;
  box-shadow: 0 0 2px rgb(70, 120, 231);
}
ul {
  text-align: left;
  margin-left: 50px;
}
.finished {
  text-decoration: line-through;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
}
</style>
