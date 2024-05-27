<script setup>
import { computed, ref, watch } from 'vue';
import TodosCreator from '../components/TodosCreator.vue';
import TodoItem from '../components/TodoItem.vue';
import { uid } from 'uid';
import { Icon } from '@iconify/vue';

const todoList = ref([]);
watch(todoList, () => {
  setTodoListLocalStorage();
},{
  deep: true
})

const todoCompleted = computed((todo) => {
  return todoList.value.every((todo) => todo.isCompleted)
}
)
const setTodoListLocalStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value));
}

const getTodoListLocalStorage = () => {
  todoList.value = JSON.parse(localStorage.getItem('todoList')) || [];
}

getTodoListLocalStorage();
const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    text: todo,
    isCompleted: null,
    isEditing: null
  });

}

const toggleComplete = (id) => {
  
  todoList.value[id].isCompleted = !todoList.value[id].isCompleted;

}

const editTodo = (todoPos) => {

  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing;

}
const deleteTodo = (todoPos) => {
  todoList.value.splice(todoPos, 1);

}

const updateTodo = (todoText, todoPos) => {
  todoList.value[todoPos].text = todoText;
  
}
</script>

<template>
  <main>
    <h1>Create Todos</h1>
    <TodosCreator @add-todo="createTodo($event)" />
    <ul class="todo-list" >
      <TodoItem 
          v-for="(todo, index) in todoList" :index="index" :todo="todo" 
          @toggle-todo="toggleComplete(index)" 
          @edit-todo="editTodo(index)" 
          @delete-todo="deleteTodo"
          @update-todo="updateTodo"
      />
    </ul>
    <p class="todos-msg" v-show="todoList.length === 0">
      <Icon icon="noto-v1:sad-but-relieved-face" width="22" />
      <span>You have no todos</span>
    </p>
    <p v-if="todoCompleted && todoList.length > 0" class="todos-msg">All todos completed <Icon icon="noto-v1:rocket" width="22"/></p>
  </main>
</template>

<style lang="scss" scoped >
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