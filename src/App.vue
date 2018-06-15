<template>
  <div id="app" class="container">
    <h1 class="text-center">Todo App</h1>
    <form @submit.prevent="addTodo">
      <div class="form-group">
        <label for="newTodo">New Todo</label>
        <input type="text" class="form-control" id="newTodo" aria-describedby="newTodoHelp" placeholder="Walk the dog..." v-model="newTodo">
        <small id="newTodoHelp" class="form-text text-muted">Enter a new todo.</small>
      </div>
      <button type="submit" class="btn btn-primary">Add Todo</button>
    </form>

    <ul class="list-group mt-3">
      <li class="list-group-item" v-for="(todo, i) in todos" :key="i">
        <button
          @click="makeDone(todo)"
          v-if="!todo.done"
          class="btn btn-primary" >Done</button>
        <button
          @click="removeTodo(i)"
          class="btn btn-danger">Delete</button>
        <span :class="{
          isDone: todo.done
        }">{{todo.title}}</span>
        </li>
    </ul>

  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      newTodo: '',
      todos: []
    }
  },
  methods:{
    addTodo(){
      this.todos.push({
        title:this.newTodo,
        done: false
      })
      this.newTodo = '';
    },
    makeDone(todo){
      todo.done = true;
    },
    removeTodo(index){
      this.todos.splice(index,1);
    }
  },
  watch:{
    todos:{
      handler(){
        localStorage.todos = JSON.stringify(this.todos)
      },
      deep: true
    }
  },
  mounted(){
    if(localStorage.todos){
      this.todos = JSON.parse(localStorage.todos)
    }
  }
}
</script>

<style>
.isDone {
  text-decoration: line-through
}
</style>
