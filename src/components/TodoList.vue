<template>
  <div class="todo-list">
    <div v-show="todos.length <= 0" class="todo-card">
      <h2>Todo List is Empty</h2>
      <p style="text-align:center;">Please <router-link to="/add-todo">Click here</router-link> to create new one</p>
    </div>
    <div class="todo-card" v-for="todo in todos" :key="todo.id">
      <h2>{{ todo.title }}</h2>
      <p>
        <strong>Description:</strong><br/> {{ todo.description }}
      </p>
      <p>
        <strong>Deadline:</strong> {{ todo.deadline }}
      </p>
      <p>
        <strong>Status:</strong> {{ todo.status }}
      </p>
      <button v-show="todo.status == 'New'" class="button-start" @click="startTodo(todo)">Start</button>
      <button v-show="todo.status == 'In-Progress'" class="button-finish" @click="finishTodo(todo)">Finish</button>
      <button class="button-delete" @click="deleteTodo(todo)">Delete</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'TodoList',
  data(){
    return {
      todos: []
    }
  },
  computed: {
  },
  methods: {
    startTodo(todo){
      const data_id = todo.id;
      todo.status = 'In-Progress';
      axios.put('https://todolist-c444d-default-rtdb.asia-southeast1.firebasedatabase.app/todos/'+data_id+'.json', todo)
        .then(response => {
          if(response.status == 200){
            console.log("Status has been updated to In-Progress");
          }
        })
        .catch(error => {
          console.log(error);
        });
    },
    finishTodo(todo){
      const data_id = todo.id;
      todo.status = 'Done';
      axios.put('https://todolist-c444d-default-rtdb.asia-southeast1.firebasedatabase.app/todos/'+data_id+'.json', todo)
        .then(response => {
          if(response.status == 200){
            console.log("Status has been updated to Done");
          }
        })
        .catch(error => {
          console.log(error);
        });
    },
    deleteTodo(todo){
      const data_id = todo.id;
      axios.delete('https://todolist-c444d-default-rtdb.asia-southeast1.firebasedatabase.app/todos/'+data_id+'.json')
        .then(response => {
          if(response.status == 200){
            console.log("Status has been deleted");
          }
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  mounted(){
    axios.get('https://todolist-c444d-default-rtdb.asia-southeast1.firebasedatabase.app/todos.json')
      .then(response => {
        for(const key in response.data){
          response.data[key].id = key;
          this.todos.push(response.data[key]);
        }
      })
      .catch(error => {
        console.log(error);
      });
  }
}
</script>

<style scoped>
.todo-list {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.todo-card {
  width: 30%;
  border: 1px solid #eaeaea;
  margin: 5px 0;
  padding: 5px 10px;
}
.todo-card p {
  text-align: justify;
}
a {
  color: #42b983;
}
button {
  border: 0;
  padding: 5px 15px;
  margin: 0 5px;
}

button:hover{
  cursor: pointer;
}

.button-start{
  color: white;
  background-color: #42b983;
}

.button-finish{
  color: white;
  background-color: #80a055;
}

.button-delete{
  color: white;
  background-color: #ff6600;
}
</style>