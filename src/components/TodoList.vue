<script setup>
defineProps({
  todos: {
    type: Array,
    default: () => [],
  },
});
const emit = defineEmits(['toggle-todo', 'delete-todo']);

const toggleTodo = (id) => {
  emit('toggle-todo', id);
};

const deleteTodo = (id) => {
  emit('delete-todo', id);
};
</script>

<template>
  <div class="todo__list">
    <div v-if="todos.length === 0" class="todo__item--empty">
      <p>할 일이 없습니다.</p>
    </div>

    <div
      v-for="todo in todos"
      class="todo__item"
      :key="todo.id"
      :class="{ 'todo__item--completed': todo.completed }"
    >
      <label class="todo__item-check-wrap">
        <input
          type="checkbox"
          :checked="todo.completed"
          @change="toggleTodo(todo.id)"
        />
        <span class="todo__item-text">{{ todo.msg }}</span>
      </label>
      <span
        class="material-symbols-outlined todo__delete-icon"
        @click="deleteTodo(todo.id)"
      >
        delete
      </span>
    </div>
  </div>
</template>
