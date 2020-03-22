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
  import { db } from '@/plugins/firebase'
  import { mapMutations } from 'vuex'

  export default {
    computed: {
      todos () {return this.$store.state.todos.list}
    },
    async mounted () {
      const snapshot = await db.collection('todos').get()
      snapshot.forEach((doc) => {
        // push処理
        this.$store.commit('todos/set', doc.data())
        // console.log(doc.data())
      });

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