<script>

export default {
  data() {
    return {
      newTodo: '',
      todos: this.getTodos(),
      editId: -1,
      todoId: this.getTodos().length ? this.getTodos().length + 1 : 0
    }
  },
  methods: {
    addTodo() {
      const todo = { id: this.todoId++, text: this.newTodo };
      this.todos.push(todo);
      this.newTodo = '';
      this.setTodos(this.todos);
    },
    removeTodo(todo) {
     this.todos = this.todos.filter(_todo => _todo != todo);
     this.setTodos(this.todos);
    },
    editTodo(todo) {
      if(this.editId != -1){
        let editTodo = this.todos.find(_todo => _todo.id == todo.id);
        editTodo.text = todo.text;
        this.editId = -1;
        this.setTodos(this.todos);
        return;
      }
      this.editId = todo.id;
    },
    setTodos(todos){
     return localStorage.setItem('todos-vue', JSON.stringify(todos))
    },
    getTodos() {
      const todos = JSON.parse(localStorage.getItem('todos-vue')) || [];
      todos.forEach((todo, i) => todo.id = i);
      this.todoId = todos.length + 1;
      return todos;
    },
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
      <div v-if="todo.id != editId">{{ todo.text }}</div>
      <div v-else> <input v-model="todo.text"></div>
      <button @click="editTodo(todo)">変更</button>
      <button @click="removeTodo(todo)">削除</button>
    </li>
  </ul>
</template>