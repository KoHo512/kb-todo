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
  <div class="todo-list">
    <div v-if="todos.length === 0" class="todo-item--empty">
      <p>할 일이 없습니다.</p>
    </div>

    <div
      v-for="todo in todos"
      class="todo-item"
      :key="todo.id"
      :class="{ 'todo-item--completed': todo.completed }"
    >
      <label class="todo-item-check-wrap">
        <input
          type="checkbox"
          :checked="todo.completed"
          @change="toggleTodo(todo.id)"
        />
        <span class="todo-item-text">{{ todo.msg }}</span>
      </label>
      <span
        class="material-symbols-outlined todo-delete-icon"
        @click="deleteTodo(todo.id)"
      >
        delete
      </span>
    </div>
  </div>
</template>
