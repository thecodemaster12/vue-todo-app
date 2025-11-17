<script setup>
import { computed, onMounted, ref, watch } from 'vue';

const todos = ref([])
const inputContent = ref('')
const username = ref('')
const filter = ref('all')

watch(username, (name) => {
  localStorage.setItem('username', name)
})

watch(todos, (t) => {
  localStorage.setItem('todos', JSON.stringify(t))
})


onMounted(() => {
  try {
    username.value = localStorage.getItem('username') || '';
    todos.value = JSON.parse(localStorage.getItem('todos')) || [];
  } catch (e) {
    todos.value = [];
  }
});

const addTodo = () => {
  if (inputContent.value.trim() === '') {
    return
  }

  todos.value.unshift({
    id: new Date().getTime(),
    content: inputContent.value,
    done: false,
  })

  inputContent.value = ''
}

const removeTodo = (todo) => {
  todos.value = todos.value.filter( t => t.id !== todo)
}

const hasTodos = computed(() => filteredData.value.length > 0)


const filteredData = computed(() => {
  switch (filter.value) {
    case 'active':
      return todos.value.filter(t => !t.done)
    case 'complete':
      return todos.value.filter(t => t.done)
    case 'all':
    default:
      return todos.value;
  }
})


</script>

<template>
<div class="container mx-auto">
  <div class="min-h-screen flex justify-center items-center">
    <div class="border-2 shadow-md shadow-black p-16 rounded-md">
      <div class="text-center border-b pb-2 mb-4">
        <h1 class="text-2xl">Welcome, 
          <input v-model="username" class="outline-0" type="text" placeholder="Name...">
        </h1>
      </div>

      <form @submit.prevent="addTodo">
        <div class="flex items-center gap-1 mb-16">
          <input v-model="inputContent" class="p-2 border w-full border-blue-500 rounded" type="text" name="" id="" placeholder="What on you mind?">
          <button class="bg-blue-500 shrink-0 text-white py-2 px-4 rounded" type="submit">Add Task</button>
        </div>
      </form>

      <div class="mb-2">
        <ul class="flex gap-4 justify-center">
          <li @click="filter = 'all'" :class="filter === 'all' ? 'active' : ''" class="py-2 px-4 rounded-full cursor-pointer border">All Tasks</li>
          <li @click="filter = 'active'" :class="filter === 'active' ? 'active' : ''" class="py-2 px-4 rounded-full cursor-pointer border">Active</li>
          <li @click="filter = 'complete'" :class="filter === 'complete' ? 'active' : ''" class="py-2 px-4 rounded-full cursor-pointer border">Completed</li>
        </ul>
      </div>

      <div class="">
        <ul>
          <li class="mb-2">Your Tasks</li>
          <li class="border border-dashed rounded p-2 text-center" v-if="!hasTodos">No Tasks</li>
          <li v-for="todo in filteredData" :key="todo.createdAt" class="flex justify-between items-center gap-2 border border-dashed rounded p-2 mb-4">
            <div class="flex gap-2 items-center">
              <input type="checkbox" v-model="todo.done" name="" id="">
              <span class="block" :class="todo.done ? 'line-through text-gray-400' : ''">{{ todo.content }}</span>
            </div>
            <button class="bg-red-500 text-white py-1 px-2 rounded" @click="removeTodo(todo.id)">Delete</button>
          </li>
        </ul>
      </div>

    </div>
  </div>
</div>
</template>

<style scoped>
.active {
  background-color: #3b82f6;
  color: white;
}
</style>
