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
  data() {
    return {
      newTodo: '',
      todos: handlingTodos.getTodos(),
      editId: -1
    }
  },
  methods: {
    addTodo() {
      const todo = { id: handlingTodos.todoId++, text: this.newTodo }
      this.todos.push(todo)
      this.newTodo = ''
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
      this.editId = todo.id;
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
  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Todoを追加する</button>    
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      <div v-if="todo.id!=editId">{{ todo.text }}</div>
      <div v-else> <input v-model="todo.text"></div>
      <button @click="editTodo(todo)">変更</button>
      <button @click="removeTodo(todo)">削除</button>
    </li>
  </ul>
</template>