<script setup>
import TodoCreator from '../components/TodoCreator.vue';
import { ref } from 'vue';
import { uid } from 'uid';

const todoList = ref([]);

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditting: null,
  });
};
</script>

<template>
  <main>
    <h1>Create Todos</h1>
    <todo-creator @create-todo="createTodo"></todo-creator>
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
}
</style>
