<script setup>
import { ref } from 'vue';

const emit = defineEmits(['add-todo']);

const inputMsg = ref('');

const addTodo = () => {
  // 데이터 전처리
  const trimmedMsg = inputMsg.value.trim();
  if (!trimmedMsg) return;

  emit('add-todo', trimmedMsg);

  inputMsg.value = '';
};
</script>

<template>
  <div class="todo-input">
    <input
      v-model="inputMsg"
      type="text"
      class="todo-input-text"
      placeholder="할 일을 입력하세요."
      @keydown.enter="addTodo"
    />
    <button class="todo-input-btn" @click="addTodo">등록</button>
  </div>
</template>
