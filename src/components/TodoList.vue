<script setup>
defineProps(['computedTodo']);

const emit = defineEmits(['update-completed', 'delete-todo']);

const updateCompleted = (id) => {
  emit('update-completed', id);
};

const deleteTodo = (id) => {
  emit('delete-todo', id);
};
</script>

<template>
  <div class="todo__list">
    <div
      class="todo__item"
      v-for="todo in computedTodo"
      :class="{ 'todo__item--completed': todo.completed }"
    >
      <input
        type="checkbox"
        :id="todo.id"
        @change="updateCompleted(todo.id)"
        :checked="todo.completed"
      />
      <label :for="todo.id" class="todo__checkbox-label">
        <span class="todo__item-text">{{ todo.msg }}</span>
      </label>
      <span
        class="material-symbols-outlined todo__delete-icon"
        @click="deleteTodo(todo.id)"
      >
        delete
      </span>
    </div>
    <div class="todo__item--no" v-if="computedTodo.length === 0">
      <p>할일 목록이 없습니다.</p>
    </div>
  </div>
</template>
