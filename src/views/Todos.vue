<template>
    <div>
        <h2>Todo application</h2>
        <router-link to="/"><h4 id="home">Home</h4></router-link>
        <AddTodo
            @add-todo="addTodo"
        />
        <select v-model="filter">
            <option value="all">All</option>
            <option value="completed">Completed</option>
            <option value="not-completed">Not Completed</option>
        </select>
        <hr>
        <Loader v-if="loading" />
        <TodoList
                v-else-if="filteredTodos.length"
                :todos="filteredTodos"
                @remove-todo="removeTodo"
        />
        <p v-else>No todos!</p>
    </div>
</template>

<script>
    import TodoList from '@/components/todoList'
    import AddTodo from '@/components/addTodo'
    import Loader from '@/components/loader'

    export default {
        name: 'Todos',
        data(){
            return {
                todos: [],
                loading: true,
                filter: 'all'
            }
        },
        mounted() {
            fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
                .then(response => response.json())
                .then(json => {
                    setTimeout(() => {
                        this.todos = json
                        this.loading = false
                    }, 2000)
                })
        },
        watch: {
            filter(value) {
                console.log(value)
            }
        },
        components: {
            TodoList,
            AddTodo,
            Loader
        },
        computed: {
          filteredTodos() {
              if (this.filter === 'all') {
                  return this.todos
              }
              if (this.filter === 'completed') {
                  return this.todos.filter(t => t.completed)
              }
              if (this.filter === 'not-completed') {
                  return this.todos.filter(t => !t.completed)
              }
          }
        },
        methods: {
            removeTodo(id) {
                this.todos = this.todos.filter(t => t.id !== id)
            },
            addTodo(todo){
                this.todos.push(todo)
            }
        }
    }
</script>

<style scoped>
    #home{
        max-resolution: 10px;
    }
</style>