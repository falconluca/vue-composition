<script>
import { ref } from 'vue'

export default {
  setup() {
    return {
      ...useTodo(),
      ...useAds(),
    }
  }
}

// 功能1：Todo列表
function useTodo() {
  const newTodo = ref('')
  const todos = ref([])

  function addNewTodo() {
    todos.value.push({
      id: Date.now(),
      content: newTodo.value,
      done: false,
    })
  }

  function markDone(todo) {
    todo.done = !todo.done
  }

  function removeTodo(index) {
    todos.value.splice(index, 1)
  }

  function removeAllTodo() {
    todos.value = []
  }

  return {
      newTodo,
      todos,
      addNewTodo,
      markDone,
      removeTodo,
      removeAllTodo,
  }
}

// 功能2：广告
function useAds() {
  const ads = ref([])

  function getAds(cnt) {
    for (let i = 0; i < cnt; i++) {
      ads.value.push({
        id: i+1,
        content: 'Ad No.' + i,
      })
    }
  }

  function removeAllAd() {
    ads.value = []
  }

  return {
    ads,
    getAds,
    removeAllAd,
  }
}
</script>

<template>
  <h1>Todo App</h1>

  <form @submit.prevent="addNewTodo">
    <label>New Todo</label>
    <input type="text" v-model="newTodo">
    <button>Submit</button>
  </form>

  <button @click="removeAllTodo">Remove All</button>
  <ul>
    <li v-for="(todo, index) in todos" :key="todo.id">
      <h3 
        :class="{done: todo.done}"
        @click="markDone(todo)"
      >{{ todo.content }}</h3>
      <button @click="removeTodo(index)">Remove</button>
    </li>
  </ul>

  <h1>Ad App</h1>
  <button @click="getAds(3)">Add Ads</button>
  <button @click="removeAllAd">Remove Ads</button>
  <ul>
    <li v-for="ad in ads" :key="ad.id">
      <h3>{{ ad.content }}</h3>
    </li>
  </ul>
</template>

<style scoped>
.done {
  color: red;
}
</style>