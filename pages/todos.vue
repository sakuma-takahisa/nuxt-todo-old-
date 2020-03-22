<template>
  <div>
    <h2>TODO</h2>
    <input @keyup.enter="addTodo" placeholder="add task">
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <input type="checkbox" :checked="todo.done" @change="toggle(todo)">
        <span :class="{done: todo.done}">{{ todo.text }}</span>
        <button @click="deleteTodo(todo)">delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
  import { mapMutations } from 'vuex'

  export default {
    computed: {
      todos () {return this.$store.state.todos.list}
    },
    mounted () {
      // console.log(this.$store.state.todos)
      // console.log(process.env.FIREBASE_API_KEY)

    },
    methods: {
      addTodo(e) {
        this.$store.commit('todos/add', e.target.value)
        e.target.value = ""
      },
      deleteTodo(todo) {
        this.$store.commit('todos/remove', todo)
      },
      ...mapMutations({
        toggle: 'todos/toggle'
      })
    }
  }
</script>

<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>