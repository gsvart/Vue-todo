<template>
  <div>
    <router-link to="/">Home</router-link>
    <hr>
    <addtodoitem v-bind:todos="todos" @add-item="addTodo"/>
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not completed</option>
    </select>
    <hr>
    <loader v-if="loading"/>
    <todolist v-else-if="filteredTodos.length" v-bind:todos="filteredTodos" @remove-item="removeTodo"/>
    <p v-else>Нет заметок</p>
  </div>
</template>

<script>
import todolist from '@/components/todolist'
import addtodoitem from '@/components/addtodoitem'
import loader from '@/components/loader'

export default {
  name: 'App',
  data() {
    return {
      todos: [],
      dbKeys: [],
      loading: true,
      filter: 'all'
    }
  },
  mounted() {
    fetch('https://questions-app-dcff9.firebaseio.com/todos.json', {
          headers: {
              'Content-type': 'application/json'
          }
      })
      .then(response => response.json())
      .then(json => {

        function dbKeysMaker(data) {
          let keysArr = []

          for (let z in data) {
            let keysItem = {
              baseKey: data[z].id,
              dbKey: z
            }

            keysArr.push(keysItem)
          }

          return keysArr
        }

        function jsonDataPrepare(data) {
          let tempArr = []

          for (let x in data) {
            tempArr.push(data[x])
          }

          return tempArr
        }
        
        setTimeout(() => {

          this.todos = jsonDataPrepare(json)
          this.dbKeys = dbKeysMaker(json)
          this.loading = false

          console.log(this.todos)
          console.log(this.dbKeys)

        }, 1000)
        
      })
  },
  computed: {
    filteredTodos() {
      if (this.filter === 'all') {
        return this.todos
        console.log('all')
      }

      if (this.filter === 'completed') {
        return this.todos.filter(item => item.completed)
        console.log('completed')
      }

      if (this.filter === 'not-completed') {
        return this.todos.filter(item => !item.completed)
        console.log('not-completed')
      }
    }
  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter(item => item.id !== id)
  
      let filteredKeys = this.dbKeys.find(item => item.baseKey == id)

      fetch(`https://questions-app-dcff9.firebaseio.com/todos/${filteredKeys.dbKey}.json`, {
          method: 'DELETE'
      })
    },
    async addTodo(newItem) {
      await this.todos.push(newItem)
      fetch('https://questions-app-dcff9.firebaseio.com/todos.json', {
          method: 'POST',
          body: JSON.stringify(newItem),
          headers: {
              'Content-type': 'application/json'
          }
      })
      .then(response => response.json())
      .then(json => {
        console.log('ALL KEYS', this.dbKeys)
        console.log('BIG DEAL', json)
        this.dbKeys.push({
          baseKey: newItem.id,
          dbKey: json.name
        })
      })
    }
  },
  components: {
    todolist,
    addtodoitem,
    loader
  }
}
</script>