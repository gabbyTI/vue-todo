<template>
  <div class="input-wrap" :class="{ 'input-err': todoState.invalid }">
    <input type="text" name="" id="" v-model="todoState.todo" />
    <todo-button @click="createTodo"></todo-button>
  </div>
  <p class="err-msg" v-show="todoState.invalid">{{ todoState.errMsg }}</p>
</template>

<script setup>
import { reactive } from 'vue';
import TodoButton from './TodoButton.vue';
const todoState = reactive({
  todo: '',
  invalid: null,
  errMsg: '',
});

const emit = defineEmits('createTodo');

function createTodo() {
  todoState.invalid = null;
  if (todoState.todo !== '') {
    emit('createTodo', todoState.todo);
    todoState.todo = '';
    return;
  }

  todoState.invalid = true;
  todoState.errMsg = 'Todo value cannot be empty';
}
</script>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &.input-err {
    border-color: red;
  }

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }
}

.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>
