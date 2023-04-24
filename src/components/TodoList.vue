<template>
  <div>
    <h2>Todo List</h2>
    <form @submit.prevent="addTodo">
      <input type="text" v-model="newTodo" placeholder="Add a new todo...">
      <button type="submit">Add</button>
    </form>
    <ul>
      <li v-for="(todo, index) in todos" :key="todo.id">
        <input type="checkbox" v-model="todo.completed" @change="updateTodo">
        <span :class="{ completed: todo.completed }">{{ todo.title }}</span>
        <button @click="deleteTodo(index)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
import {ref} from 'vue'

export default {
  setup() {
    // 初始化待办事项列表
    const todos = ref([
      {id: 1, title: 'Learn Vue 3', completed: false},
      {id: 2, title: 'Build a Todo app', completed: true}
    ])

    // 初始化新待办事项
    const newTodo = ref('')

    // 添加新待办事项
    const addTodo = () => {
      if (newTodo.value.trim() !== '') {
        todos.value.push({id: Date.now(), title: newTodo.value, completed: false})
        newTodo.value = ''
      }
    }

    // 更新待办事项完成状态
    const updateTodo = () => {
      localStorage.setItem('todos', JSON.stringify(todos.value))
    }

    // 删除待办事项
    const deleteTodo = (index) => {
      todos.value.splice(index, 1)
    }

    return {
      todos,
      newTodo,
      addTodo,
      updateTodo,
      deleteTodo
    }
  }
}
</script>

<style>
.completed {
  text-decoration: line-through;
}
</style>