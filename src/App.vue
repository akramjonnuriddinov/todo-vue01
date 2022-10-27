<script setup>
import PenIcon from './components/icons/PenIcon.vue'
import XmarkIcon from './components/icons/XmarkIcon.vue'
import { ref, onMounted, watch } from 'vue'

const input = ref('')
const todos = ref([])
let disabled = ref(1)

const addBtn = () => {
  if(input.value !== '') {
    todos.value.push(input.value)
    input.value = ''
    disabled.value = 1
  }
}

const removeBtn = (todo) => {
  todos.value = todos.value.filter((i) => i !== todo)
}

const editBtn = () => {
  disabled.value = (disabled.value + 1) % 2
}

watch(todos, (newValue) => {
  localStorage.setItem('todos', JSON.stringify(newValue))
}, {
  deep: true
})


onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem('todos'))
})
</script>

<template>
  <div class="container max-w-2xl px-5 py-5 mx-auto my-5 bg-white">
    <h1 class="mb-4 text-2xl font-bold text-center">TODO LIST</h1>
    <div class="flex justify-between mb-8">
      <input v-model="input" class="w-full px-2 py-2 mr-4 text-xl border" type="text" placeholder="Enter the task">
      <button @click="addBtn" class="px-4 text-3xl text-white bg-blue-500 hover:bg-blue-400">+</button>
    </div>
    <ul>
      <li v-for="todo in todos" class="flex items-center justify-between px-2 py-2 border-t border-b hover:bg-sky-100">
        <label for="checked" class="flex items-center w-full cursor-pointer">
          <input class="mr-4" type="checkbox" name="checked">
          <input class="w-full text-xl text-gray-800 bg-transparent outline-none" :disabled="disabled == 1" type="text" :value="todo">
        </label>
        <div class="flex items-center">
          <button @click="disabled = (disabled + 1) % 2" class="mr-4">
            <PenIcon class="w-4 text-sky-700" />
          </button>
          <button @click="removeBtn(todo)">
            <XmarkIcon class="w-4 text-sky-700" />
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<style>
body {
  background-color: rgb(35, 162, 235);
}

input:checked + input {
  text-decoration: line-through;
}
</style>