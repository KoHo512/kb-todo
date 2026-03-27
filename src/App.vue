<script setup>
import { ref, computed, watch, onMounted } from 'vue';

import TodoHeader from './components/TodoHeader.vue';
import TodoList from './components/TodoList.vue';
import TodoInput from './components/TodoInput.vue';

const todos = ref([]);
const current = ref('all');

const addTodo = (inputMsg) => {
  const item = {
    id: crypto.randomUUID(),
    msg: inputMsg,
    completed: false,
  };
  todos.value.push(item);
};

const updateTab = (tab) => {
  current.value = tab;
};

const updateCompleted = (id) => {
  const idx = todos.value.findIndex((todo) => todo.id === id);
  todos.value[idx].completed = !todos.value[idx].completed;
};

const deleteTodo = (id) => {
  const idx = todos.value.findIndex((todo) => todo.id === id);
  todos.value.splice(idx, 1);
};

const computedTodo = computed(() => {
  if (current.value === 'completed') {
    return todos.value.filter((todo) => todo.completed);
  } else {
    return todos.value;
  }
});
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
