<script setup>
  import Task from '../components/Task.vue';
  import Alert from '../components/Alert.vue'
  import axios from 'axios'
  import { ref } from 'vue'

  const task = ref('')
  const showAlert = ref(false)
  const textAlert = ref('')
  const todos = ref([])
  const url = 'https://todolist-vue-cc78d-default-rtdb.firebaseio.com'

  axios.get(`${url}/todo.json`)
  .then((response) => {
    todos.value = response.data
  })
  .catch((error) => { 
    console.log(error);
  })

  function addTodo(){
    const todo = {
      title: task.value,
      completed: false
    }
    axios.post(`${url}/todo.json`, todo)
    .then((response) => {
      console.log(response.data);
      todos.value = {...todos.value, [response.data.name]: todo}
      textAlert.value = "Você adicionou uma tarefa"
      showAlert.value = true
      setTimeout(() =>{
        showAlert.value = false
      }, 2000)
    })
    .catch((error) => {
      console.log(error);
    })
  }
  function onUpdate(dados){
    const { item, name } = dados

    const todo = {
      ...item,
      completed: !item.completed
    }
    axios.put(`${url}/todo/${name}.json`, todo)
    .then((response) => {
      todos.value = {...todos.value, [name]: response.data}
      textAlert.value = todo.completed ? "Você concluiu esta tarefa!" : "Você não concluiu esta tarefa"
      showAlert.value = true
      setTimeout(() =>{
        showAlert.value = false
      }, 2000) 
    })
    .catch((error) => {
      console.log(error);
    })
  }
  function onDelete(dados){
    const { name } = dados

    axios.delete(`${url}/todo/${name}.json`)
    .then((response) => {
      const keys = Object.keys(todos.value)?.filter(nameTodo => nameTodo !== name)
      const filterTodo = keys.reduce((acc, key) => {
        acc[key] = { ...todos.value[key] };
        return acc;
      }, {})
      todos.value = filterTodo
      textAlert.value = "Você excluiu uma tarefa"
      showAlert.value = true
      setTimeout(() =>{
        showAlert.value = false
      }, 2000) 
    })
    .catch((error) => {
      console.log(error);
    })
  }
  function onClose(){
    console.log('close');
    showAlert.value = false
  }

</script>

<template>
  <main>
    <h1>To-do List</h1>
    <div class="input-task">
      <input type="text" v-model="task" >
      <button @click="addTodo">Criar</button>
    </div>
    <Alert variant="success" :text="textAlert" @close="onClose()" v-if="showAlert"/>
    <div v-for="(item, name) in todos" :key="name">
      <Task @update="onUpdate" @delete="onDelete" :item="item" :name="name"/>
    </div>
  </main>
</template>

<style scoped>
main{
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  display: flex;
  align-items: center;
  flex-direction: column;
}
main .input-task{
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 5px;
}
main .input-task input{
  padding: 10px;
  border: 1px #242424 solid;
  border-radius: 10px;
  width: 250px;
}
main .input-task button{
  padding: 10px;
  border: 1px rgb(54, 8, 219) solid;
  border-radius: 10px;
  background-color:rgb(54, 8, 219) ;
  color: #fff;
  cursor: pointer;
  transition: 0.5s all;
}
main .input-task button:hover{
  border: 1px rgb(38, 4, 163) solid;
  background-color: rgb(38, 4, 163) ;
}
</style>
