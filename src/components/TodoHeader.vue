<script setup>
defineProps({
  current: {
    type: String,
    default: 'all',
  },
});
const emit = defineEmits(['update-tab']);

const updateTab = (tab) => {
  emit('update-tab', tab);
};
</script>

<template>
  <div class="todo-title">
    <h1 class="todo-text">Todo List</h1>
    <ul class="todo-tab">
      <li
        :class="{ 'todo-tab--active': current === 'all' }"
        @click="updateTab('all')"
      >
        전체
      </li>
      <li
        :class="{ 'todo-tab--active': current === 'completed' }"
        @click="updateTab('completed')"
      >
        완료
      </li>
    </ul>
  </div>
</template>
