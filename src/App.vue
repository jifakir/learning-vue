<script setup>
import {ref, watch, reactive, onMounted} from 'vue';

const todo = ref('');
const state = reactive({todos: []});
  const addTodo = () => {
    state.todos.push(todo.value);
    const store = JSON.stringify(state.todos);
    localStorage.setItem('todos', store);
    return todo.value = '';
  }
  const removeTodo = (index) => {
        const newState = state.todos.filter((item, idx) => idx !== index);
        state.todos = newState;
        localStorage.setItem('todos', JSON.stringify(state.todos));
    }

  onMounted(() => {
    if(localStorage.getItem('todos')){
      const storage = JSON.parse(localStorage.getItem('todos'));
      state.todos = storage;
      console.log(storage);
    }
  })
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
            <li v-if="state.todos.length === 0" class="px-5 py-3 bg-gray-200 bg-opacity-30 rounded shadow-md my-2 flex justify-between" >
                <p class="">Please, add item to see here.</p>
            </li>
            <li class="px-5 py-3 bg-gray-200 bg-opacity-30 rounded shadow-md my-2 flex justify-between" v-for="(item, idx) in state.todos" :key="idx">
                <p class="">{{item}}</p>
                <div class="cursor-pointer font-medium" @click="removeTodo(idx)">
                    <svg stroke="currentColor" fill="currentColor" stroke-width="0" viewBox="0 0 1024 1024" height="1em" width="1em" xmlns="http://www.w3.org/2000/svg"><path d="M360 184h-8c4.4 0 8-3.6 8-8v8h304v-8c0 4.4 3.6 8 8 8h-8v72h72v-80c0-35.3-28.7-64-64-64H352c-35.3 0-64 28.7-64 64v80h72v-72zm504 72H160c-17.7 0-32 14.3-32 32v32c0 4.4 3.6 8 8 8h60.4l24.7 523c1.6 34.1 29.8 61 63.9 61h454c34.2 0 62.3-26.8 63.9-61l24.7-523H888c4.4 0 8-3.6 8-8v-32c0-17.7-14.3-32-32-32zM731.3 840H292.7l-24.2-512h487l-24.2 512z"></path></svg>
                </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

