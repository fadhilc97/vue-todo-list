<template>
  <div class="about">
    <h1>Add New Todo</h1>
    <form>
      <div class="form-group">
        <label for="title">Title</label>
        <input id="title" type="text" v-model="todoItem.title" placeholder="Input title"/>
      </div>
      <div class="form-group">
        <label for="deadline">Deadline</label>
        <input id="deadline" type="date" v-model="todoItem.deadline"/>
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <textarea id="description" v-model="todoItem.description" cols="30" rows="10" placeholder="Input Description"></textarea>
      </div>
      <button type="submit" @click.prevent="createTodo">Create Todo</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'AddTodo',
  data(){
    return {
      todoItem: {
        title: null,
        deadline: null,
        description: null,
        status: 'New'
      }
    }
  },
  methods: {
    createTodo(){
      const deadlineFormatted = new Date(this.todoItem.deadline).toLocaleDateString('id-ID');
      this.todoItem.deadline = deadlineFormatted;
      axios.post('https://todolist-c444d-default-rtdb.asia-southeast1.firebasedatabase.app/todos.json', this.todoItem)
        .then(response => {
          if (response.status == 200) {
            console.log('Data has been added');
            return this.$router.push('/');
          }
        })
        .catch(error => {
          console.log(error);
        });
    }
  }
}
</script>

<style scoped>
.about {
  display: flex;
  flex-direction: column;
  align-items: center;
}

form {
  text-align: left;
}
form button {
  border: 0;
  padding: 5px 15px;
  background-color: #42b983;
  color: white;
}

form button:hover{
  cursor: pointer;
}

.form-group {
  margin: 15px 0;
}
.form-group label {
  display: block;
  margin-bottom: 5px;
}
.form-group input[type='text'],
.form-group input[type='date'],
.form-group textarea{
  width: 100%;
  padding: 5px;
}
</style>
