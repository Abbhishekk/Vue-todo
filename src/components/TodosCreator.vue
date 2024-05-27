<script setup>
import { defineEmits, reactive } from 'vue';
import TodoButton from './TodoButton.vue';
const todoState = reactive({
    todos: "",
    invalid: null,
    errMsg: "",

});
const emit = defineEmits(['add-todo'])
const createTodo = () => {
    if(todoState.todos !== ""){
        todoState.invalid = null;
        todoState.errMsg = "";
        emit('add-todo',todoState.todos);
        todoState.todos = "";
        return;
    }
    else{
        todoState.invalid = true;
        todoState.errMsg = "Please enter a todo";
    }
   
}
</script>

<template>
    <div class="input-wrap" :class="{'input-err': todoState.invalid}">
        <input type="text" v-model="todoState.todos" placeholder="Create a new todo..." />
        <TodoButton @click="createTodo" />
    </div>
    <p class="err-msg" v-show="todoState.invalid" >{{ todoState.errMsg }}</p>
</template>



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

  button {
    padding: 8px 16px;
    border: none;
  }
}

.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>