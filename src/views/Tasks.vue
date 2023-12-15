<template>
  <main>
    <h1>Tarefas Concluidas</h1>
    <Alert variant="success" :text="textAlert" @close="onClose()" v-if="showAlert"/>
    <div v-for="(item, name) in todos" :key="name">
      <Task @update="onUpdate" @delete="onDelete" :item="item" :name="name"/>
    </div>
  </main>
</template>

<script setup>
  import Task from '../components/Task.vue';
  import Alert from '../components/Alert.vue'
  import axios from 'axios'
  import { ref } from 'vue'

  const todos = ref([])
  const showAlert = ref(false)
  const textAlert = ref('')
  const url = 'https://todolist-vue-cc78d-default-rtdb.firebaseio.com'

  axios.get(`${url}/todo.json`)
  .then((response) => {
    const keys = Object.keys(response.data)?.filter(name => response.data[name]?.completed)
    const completeTodo = keys.reduce((acc, key) =>{
      acc[key] = { ...response.data[key]};
      return acc;
    }, {})
    todos.value = completeTodo
    console.log(completeTodo);
  })
  .catch((error) => { 
    console.log(error);
  })

  function onUpdate(dados){
    const { item, name } = dados

    const todo = {
      ...item,
      completed: !item.completed
    }
    axios.put(`${url}/todo/${name}.json`, todo)
    .then((response) => {
      const newTodos = {...todos.value, [name]: response.data}
      const keys = Object.keys(newTodos)?.filter(name => newTodos[name]?.completed)
      const completeTodo = keys.reduce((acc, key) =>{
        acc[key] = { ...newTodos[key]};
        return acc;
      }, {})
      todos.value = completeTodo
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

</script>

<style scoped>
main{
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  display: flex;
  align-items: center;
  flex-direction: column;
}
</style>
