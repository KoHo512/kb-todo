<script setup>
import { ref, computed, watch, onMounted } from 'vue';

import TodoHeader from './components/TodoHeader.vue';
import TodoList from './components/TodoList.vue';
import TodoInput from './components/TodoInput.vue';

const todos = ref([]);
const current = ref('all');

const addTodo = (trimmedMsg) => {
  const item = {
    id: crypto.randomUUID(),
    msg: trimmedMsg,
    completed: false,
  };

  // 새롭게 입력된 할 일을 배열 맨 앞에 추가 (최신 등록 할 일이 맨 위에 렌더링)
  todos.value.unshift(item);
};

const updateTab = (tab) => {
  current.value = tab;
};

const toggleTodo = (id) => {
  // const idx = todos.value.findIndex((todo) => todo.id === id);
  // todos.value[idx].completed = !todos.value[idx].completed;

  todos.value = todos.value.map((todo) => {
    if (todo.id === id) {
      return { ...todo, completed: !todo.completed };
    } else {
      return todo;
    }
  });
};

const deleteTodo = (id) => {
  // const idx = todos.value.findIndex((todo) => todo.id === id);
  // todos.value.splice(idx, 1);

  todos.value = todos.value.filter((todo) => todo.id !== id);
};

// 현재 탭 상태에 따라 할 일 리스트 필터
const filteredTodo = computed(() => {
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
      :todos="filteredTodo"
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />

    <TodoInput @add-todo="addTodo" />
  </div>
</template>
