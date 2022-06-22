<script>
const handlingTodos = {
  setTodos: function(todos){
    return localStorage.setItem('todos-vue', JSON.stringify(todos))
  },
  getTodos: function() {
    const todos = JSON.parse(localStorage.getItem('todos-vue')) || [];
    todos.forEach((todo, i) => todo.id = i);
    handlingTodos.todoId = todos.length + 1;
    return todos;
  },
}

export default {
  emits: ['addNew'],
  data() {
    return {
      newTodo: '',
      todos: handlingTodos.getTodos(),
      editId: -1,
      newButtonClicked: false,
      viewTodoObj: null,
      newAdded: false
    }
  },
  methods: {
    addTodo() {
      this.$emit('addNew');
    },
    removeTodo(todo) {
     this.todos = this.todos.filter(_todo => _todo != todo);
    },
    editTodo(todo) {
      if(this.editId != -1){
        let editTodo = this.todos.find(_todo => _todo.id == todo.id);
        editTodo.text = todo.text;
        return this.editId = -1;
      }
            console.log(todo);

      this.editId = todo.id;
    },
    viewTodo(todo){
      this.viewTodoObj = todo;
      console.log(this.viewTodoObj.id)
    }
  },
  watch: {
    todos: {
      handler: function(todos){
        handlingTodos.setTodos(todos)
      },
      deep: true
    }
  }
}
</script>

<template>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      <a href="#" @click="viewTodo(todo)" v-if="todo.id!=editId">{{ todo.text.split("\n")[0] }}</a>
      <div v-else> <input v-model="todo.text"></div>
    </li>
  </ul>
  <div class="box" v-if="viewTodoObj">
  <textarea class="text" v-model="viewTodoObj.text" :disabled="editId != viewTodoObj.id"/>
    <button id="edit" @click="editTodo(todo)">編集</button>
    <button id="remove" @click="removeTodo(todo)">削除</button>
  </div>
  <button @click="addTodo">+</button>
</template>

<style>
  .box{
  width: 300px;
  height: 600px;
  border: 1px solid black;
  box-sizing: border-box;
  float:right;
  position: relative;
  }
  .text {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    right: 0;
  }
  #edit {
    position: absolute;
    bottom: 0;
    left: 60px;
  }
  #remove {
    position: absolute;
    bottom: 0;
    left: 100px
  }
</style>