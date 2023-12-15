<script setup>
import { RouterLink, RouterView } from 'vue-router'

const showLink = true;

</script>

<template>
  <main id="main-app">
    <header>
        <nav>
          <RouterLink v-show="showLink" to="/">Criar tarefa</RouterLink>
          <RouterLink to="/tasks">Ver tarefas concluidas</RouterLink>
        </nav>
    </header>
  
    <RouterView />
  </main>
</template>

<style scoped>
#main-app{
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  align-items: center;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
header {
  line-height: 1.5;
  display: flex;
}

nav {
  text-align: center;
  margin-top: 2rem;
  display: flex;
  gap: 20px;
}

nav a.router-link-exact-active {
  color: rgb(54, 8, 219) ;
  background-color: rgba(54, 8, 219, 0.123);
  border: 1px solid rgb(54, 8, 219);
  border-radius: 10px;
}
nav a {
  text-decoration: none;
  color: rgb(54, 8, 219);
  padding: 5px 10px;
  transition: 0.5s all;
  border: 1px solid rgba(54, 8, 219, 0);
  border-radius: 10px;
}
nav a:hover {
  color: rgb(54, 8, 219);
  background-color: rgba(54, 8, 219, 0.123);
  border: 1px solid rgb(54, 8, 219);
  border-radius: 10px;
}
</style>
