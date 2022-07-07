<script>
export default {
  emits: ["addNew"],
  data() {
    return {
      newTodo: "",
      todos: this.getTodos(),
      editedTodo: null,
      newButtonClicked: false,
      viewTodoObj: null,
      newAdded: false,
    };
  },
  methods: {
    setTodo(todos) {
      return localStorage.setItem("todos-vue", JSON.stringify(todos));
    },
    getTodos() {
      const todos = JSON.parse(localStorage.getItem("todos-vue")) || [];
      this.todoId = todos.length + 1;
      return todos;
    },
    addTodo() {
      const todo = { id: this.todoId++, text: "新規メモ" };
      this.todos.push(todo);
      this.viewTodoObj = todo;
      this.editedTodo = todo;
    },
    removeTodo() {
      this.todos = this.todos.filter((_todo) => _todo != this.viewTodoObj);
      this.setTodo(this.todos);
    },
    editTodo() {
      if (this.editedTodo) {
        let editTodo = this.todos.find(
          (_todo) => _todo.id == this.viewTodoObj.id
        );
        editTodo.text = this.viewTodoObj.text;
        this.setTodo(this.todos);
        return (this.editedTodo = null);
      }
      this.editedTodo = this.viewTodoObj;
    },
    viewTodo(todo) {
      this.viewTodoObj = todo;
    },
  },
};
</script>

<template>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      <a href="#" @click="viewTodo(todo)" v-if="todo != editedTodo">{{
        todo.text.split("\n")[0]
      }}</a>
      <div v-else><input v-model="todo.text" /></div>
    </li>
  </ul>
  <div class="box" v-if="viewTodoObj">
    <textarea
      class="text"
      v-model="viewTodoObj.text"
      :disabled="editedTodo !== viewTodoObj"
    />
    <button id="edit" @click="editTodo()">編集</button>
    <button id="remove" @click="removeTodo()">削除</button>
  </div>
  <button @click="addTodo">+</button>
</template>

<style>
.box {
  width: 300px;
  height: 600px;
  border: 1px solid black;
  box-sizing: border-box;
  float: right;
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
  left: 100px;
}
</style>
