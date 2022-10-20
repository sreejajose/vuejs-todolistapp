<script setup>
import HelloWorld from './components/HelloWorld.vue'
import { ref, onMounted, computed, watch } from 'vue'
const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

const addTodo = () => {
  if(input_content.value.trim() === '' || input_category.value === null){
    return
  }
  
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })
}

const removeTodo = todo => {
  todos.value = todos.value.filter(t => t !== todo)
}

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {deep: true})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

</script>

<template>
  <HelloWorld msg="To-do App" />
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Whats up, <input type="text" placeholder="Name here" v-model="name" class="text-box-style"/>
      </h2>

      <section class="create-todo">
        <h3>
          CREATE A TODO
        </h3>
        <form @submit.prevent="addTodo">
          <h5 class="todo-text">Add an entry to your Todo List: </h5>
          <input type="text" placeholder="eg make a video" v-model="input_content" class="text-box-style"/>
        
          <h5>Pick a category</h5>
          <div class="options">
            <label class="display-block">
              <input type="radio" class="radio-style" name="category" value="business" v-model="input_category"/>
              <span class="bubble business"></span>
              <div class="display-inline">Business</div>
            </label>
            
            <label class="display-block">
              <input type="radio" class="radio-style" name="category" value="personal" v-model="input_category"/>
              <span class="bubble personal"></span>
              <div class="display-inline">Personal</div>
            </label>
          </div>
          <button type="submit" value="Add todo" class="btn">Add Todo Item</button>
        </form>
      </section>
    </section>

    <section class="todo-list">
      <h3>TODO ITEMS</h3>
      <div class="list">
        <div v-for="todo in todos_asc">
          <label>
            <input type="checkbox" class="radio-style" v-model="todo.done" />
          </label>

          <div class="todo-content  display-inline">
            <input type="text" v-model="todo.content" class="text-box-style"/>
          </div>

          <div class="actions display-inline">
            <button class="btn" @click="removeTodo(todo)">Remove</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<style scoped>

</style>
