<template>
  <div class="app-container">
    <h1 class="app-title">ToDo List</h1>
    <div class="app-box">
      <form class="form-add" @submit="addTodo">
        <input type="text" v-model="text" ref="input">
        <button>Add</button>
      </form>
      <h3>{{numberTodo}}/{{todos.length}} is completed</h3>
      <div class="list-todo"  v-if="todos.length > 0">
        <TodoItem 
        v-for="todo in todos" :data="todo" 
        :key="todo.id"
        @toggle="onToggle"
        @deleteTodo="onDeleteTodo"
        ></TodoItem>
      </div>
    </div>
  </div>
</template>

<script>
import TodoItem from './components/TodoItem.vue'
import { v4 as uuidv4 } from 'uuid';

export default {
  name: 'App',
  components: {
    TodoItem
  },
  data(){
    return{
      text: '',
      todos: []
    }
  },
  mounted(){
    const todoStorageJSON = localStorage.getItem('todos')
    if(todoStorageJSON){
      this.todos = JSON.parse(todoStorageJSON)
    }
  },
  computed: {
    numberTodo(){
      const x = this.todos.filter(el => el.isComplete === true)
      return x.length
    }
  },
  methods: {
    addTodo(e){
      e.preventDefault()
      const todoStorageJSON = localStorage.getItem('todos')
      const todoStorage = JSON.parse(todoStorageJSON)
      const newTodos = !todoStorage ? [{
        id: uuidv4(),
        isComplete: false,
        title: this.text
      }] :  [...todoStorage, {
        id: uuidv4(),
        isComplete: false,
        title: this.text
      }]
      localStorage.setItem('todos', JSON.stringify(newTodos))
      this.todos = newTodos
      this.text=''
      this.$refs.input.focus()
    },
    onToggle(data){
      this.todos.forEach(el => {
        if(el.id === data.id){
          el.isComplete = data.isComplete
        }
      });
      localStorage.setItem('todos', JSON.stringify(this.todos))
    },
    onDeleteTodo(id){
      this.todos = this.todos.filter(el=> {
        return el.id !== id
      })
      localStorage.setItem('todos', JSON.stringify(this.todos))

    }
  }
}
</script>

<style>
body{
  margin: 0;
  padding: 0;
}
/* .app-container{
  text-align: center;
} */
.app-title{
  background-color: #ccc;
  padding: 50px 0;
  margin: 0;
  text-align: center;
}

.app-box{
  padding: 0 50px;

}
.form-add{
  margin: 20px 0;
  display:flex
}
.form-add > input{
  flex-grow: 1;
  border-radius: 5px;
  border: 1px solid #ccc;
  outline: none;
}
.form-add > button{
  padding: 10px 20px;
  margin-left: 10px;
  background-color: red;
  border-radius: 5px;
  color: #fff;
  border: 1px solid #ccc;
  cursor: pointer;
  outline: none;
}
/* .list-todo{
  padding: 10px;
  border: 1px solid #ccc;
} */
</style>
