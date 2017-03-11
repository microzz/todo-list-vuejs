<template>
  <div id="app">
    <div class="container">
      <h1 v-text="text"></h1>
      <input type="text" v-model="newItem" v-on:keyup.enter="addNew" />
      <button v-on:click="addNew">Add</button>
      <ul>
        <li v-for="(item, index) in items">
          <span v-bind:class="{finished: item.isFinished}">{{item.label}}</span>
          <span v-on:click="toggle(item)" class="func first">{{!item.isFinished ? 'done' : 'todo'}}</span>
          <span v-on:click="items.splice(index, 1)" class="func">delete</span>
        </li>
      </ul>
      <div v-show="hasData" v-on:click="del" class="delAll">
        Delete All
      </div>
    </div>
  </div>
</template>

<script>
import Store from './store';

export default {
  name: 'app',
  mounted() {
    this.hasData = this.items && this.items.length ? true : false;
  },
  data() {
    return {
      text: 'Todo List',
      items: Store.fetch(),
      newItem: '',
      hasData: false
    }
  },
  watch: {
    items: {
      handler(items) {
        Store.save(items);
        this.hasData = this.items && this.items.length ? true : false;
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
      if (!this.items) {
        this.items = []
      }
      this.items.push({label: this.newItem, isFinished: false});
      this.newItem = '';
    },
    del() {
      this.items = null;
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
li {
  margin-top: 9px;
}
button {
  border-radius: 14px;
  color: black;
  box-shadow: 0 0 2px rgb(70, 120, 231);
  background: none;

}
span.func {
  cursor: pointer;
  margin-left: 7px;
  text-decoration: underline;
}
span.func.first {
  margin-left: 13px;
}
ul {
  text-align: left;
  margin-left: 50px;
}
.finished {
  text-decoration: line-through;
}
div.delAll {
  text-decoration: underline;
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
