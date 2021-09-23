<script setup>
import {ref, watch, reactive, onMounted} from 'vue';
import Todo from './components/Todo.vue';

const todo = ref('');
const state = reactive({todos: []});
  const addTodo = () => {
    state.todos.push(todo.value);
    const store = JSON.stringify(state.todos);
    localStorage.setItem('todos', store);
    return todo.value = '';
  }
  onMounted(() => {
    if(localStorage.getItem('todos')){
      const storage = JSON.parse(localStorage.getItem('todos'));
      state.todos = storage;
      console.log(storage);
    }
  })
  // watch(
  //   () => state.todos,
  //   (todos, prevTodos) => {
  //     localStorage.setItem('todos', todos)
  //   }
  // )
</script>

<template>
  <div class="min-h-screen max-h-screen w-full flex justify-center items-center overflow-hidden bg-gradient-to-br from-indigo-400 to-green-400">
    <div class="w-5/6 h-96 text-yellow-500 text-md font-semibold tracking-wider bg-gray-900 rounded-lg bg-opacity-60 shadow-lg px-5 py-2">
      <h1 class="w-full text-3xl font-bold text-center uppercase">To do</h1>
      <div class="w-full min-h-full flex justify-evenly">
        <div class="w-1/2 pr-5">
          <h1 class="text-center text-xl mb-5">Add Todo</h1>
          <form  class="w-full" @submit.prevent="addTodo">
            <label for="addTodo">
              <input type="text" v-model="todo" id="addTodo" class="px-5 py-3 w-full focus:outline-none rounded bg-gray-200 bg-opacity-20 font-medium shadow-inner placeholder-yellow-400" placeholder="What's in your mind...">
            </label>
            <div class="w-full flex justify-center">
              <button type="submit" class="bg-gray-900 shadow-md w-52 py-2 my-5 rounded bg-opacity-30 font-bold uppercase">Add</button>
            </div>
          </form>
        </div>
        <div class="pl-5 w-1/2">
          <h1 class="text-center mb-5 text-xl">Todo Lists</h1>
          <ul class="w-full h-72 pl-3 pr-4 rounded overflow-y-auto shadow-inner scrollbar scrollbar-thin scrollbar-thumb-yellow-500 scrollbar-thumb-rounded-full scrollbar">
            <!-- <li class="px-5 py-3 bg-gray-200 bg-opacity-30 rounded shadow-md" v-if="state.todos.length <= 0">Please, add something in todo list.</li>
            <li class="px-5 py-3 bg-gray-200 bg-opacity-30 rounded shadow-md my-2 flex justify-between" v-for="(item, idx) in state.todos" :key="idx">
              <p class="">{{item}}</p>
              <div class="">icons</div>
            </li> -->
            <Todo :todos="state.todos"></Todo>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

