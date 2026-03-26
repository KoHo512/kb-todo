<script>
import TodoHeader from './components/TodoHeader.vue';
import TodoInput from './components/TodoInput.vue';
import TodoList from './components/TodoList.vue';

export default {
  components: {
    TodoHeader,
    TodoList,
    TodoInput,
  },
  data() {
    return {
      todos: [],
      current: 'all',
    };
  },
  methods: {
    addTodo(inputMsg) {
      const item = {
        id: crypto.randomUUID(),
        msg: inputMsg,
        completed: false,
      };
      this.todos.push(item);
    },
    updateTab(tab) {
      this.current = tab;
    },
    updateCompleted(id) {
      const idx = this.todos.findIndex((todo) => todo.id === id);
      this.todos[idx].completed = !this.todos[idx].completed;
    },
    deleteTodo(id) {
      const idx = this.todos.findIndex((todo) => todo.id === id);
      this.todos.splice(idx, 1);
    },
  },
  computed: {
    computedTodo() {
      if (this.current === 'completed') {
        return this.todos.filter((todo) => todo.completed);
      } else {
        return this.todos;
      }
    },
  },
};
</script>

<template>
  <div class="todo">
    <TodoHeader :current="current" @update-tab="updateTab" />
    <TodoList
      :computed-todo="computedTodo"
      @update-completed="updateCompleted"
      @delete-todo="deleteTodo"
    />
    <TodoInput @add-todo="addTodo" />
  </div>
</template>
