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

// 완료된 할 일 개수 계산
const completedCount = computed(() => {
  return todos.value.filter((todo) => todo.completed).length;
});

// 완료되지 않은 할 일 개수 계산
const remainingCount = computed(() => {
  return todos.value.filter((todo) => !todo.completed).length;
});

// 완료된 할 일 삭제
const deleteCompleted = () => {
  todos.value = todos.value.filter((todo) => !todo.completed);
};
</script>

<template>
  <div class="todo">
    <TodoHeader :current="current" @update-tab="updateTab" />

    <div class="todo-summary">
      <p>전체 할 일 : {{ todos.length }}개</p>
      <div class="todo-summary-wrap">
        <p>완료한 할 일 : {{ completedCount }}개</p>
        <p>남은 할 일 : {{ remainingCount }}개</p>
      </div>
    </div>

    <div class="todo-manage">
      <button :disabled="!completedCount" @click="deleteCompleted">
        완료한 할 일 전체 삭제
      </button>
    </div>

    <TodoList
      :todos="filteredTodo"
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />

    <TodoInput @add-todo="addTodo" />
  </div>
</template>
