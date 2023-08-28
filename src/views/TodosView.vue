<template>
  <main>
    <h1>Create Todos</h1>
    <todo-creator @create-todo="createTodo"></todo-creator>
    <ul class="todo-list" v-if="todoList.length > 0">
      <todo-item
        v-for="(todo, index) in todoList"
        :todo="todo"
        :key="index"
        :index="index"
        @mark-task="markTask"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      ></todo-item>
    </ul>
    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" color="#41b080" width="22" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
    <p class="todos-msg" v-if="todoCompleted && todoList.length > 0">
      <Icon icon="noto-v1:party-popper" />
      <span> You have completed all your todos !</span>
    </p>
  </main>
</template>

<script setup>
import { ref, watch, computed } from 'vue';
import { Icon } from '@iconify/vue';
import TodoCreator from '../components/TodoCreator.vue';
import TodoItem from '../components/TodoItem.vue';
import { uid } from 'uid';

const todoList = ref([]);

watch(
  todoList,
  () => {
    setTodoListLocalStorage();
  },
  {
    deep: true,
  }
);

const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted);
});

const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem('todoList'));
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
};

fetchTodoList();

const setTodoListLocalStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value));
};

const markTask = (index) => {
  todoList.value[index].isCompleted = !todoList.value[index].isCompleted;
};

const toggleEditTodo = (index) => {
  todoList.value[index].isEditting = !todoList.value[index].isEditting;
};

const updateTodo = (index, todoVal) => {
  todoList.value[index].todo = todoVal;
};

const deleteTodo = (index) => {
  todoList.value.splice(index, 1);
  // todoList.value = todoList.value.filter((todo) => {
  //   todo.id !== todoId;
  // });
};

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditting: null,
  });
};
</script>

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

  ul {
    display: block;
    list-style-type: disc;
    margin-block-start: 1em;
    margin-block-end: 1em;
    margin-inline-start: 0px;
    margin-inline-end: 0px;
    padding-inline-start: 0px;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
