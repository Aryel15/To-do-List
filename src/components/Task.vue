<script setup>
  import { toRefs } from "vue"
  const props = defineProps({
    item: Object,
    name: String
  });
  const { item, name } = toRefs(props)
  const emit = defineEmits(['update', 'delete'])
  function onUpdate(item, name){
    emit('update', {
      item: item,
      name: name
    })
  }
  function onDelete(name){
    emit('delete', {
      name: name
    })
  }
</script>

<template>
    <div class="todos-item">
        <input 
          v-model="item.completed"
          @click="onUpdate(item, name)"
          type="checkbox" 
          name="completed"
          :id="name">
        <label :for="name" :class="{'completed':item.completed, 'no-completed': !item.completed}">
            &#10004;
        </label>
        <span>
            {{ item?.title }}
        </span>
        <i class="fa-regular fa-trash-can" @click="onDelete(name)"></i>
    </div>
</template>

<style scoped>
  .todos-item{
    background-color: rgb(241, 241, 241);
    margin: 10px;
    padding: 15px 20px;
    width: 300px;
    display: flex;
    gap: 15px;
    align-items: center;
    justify-content: flex-start;
    border-bottom: 4px solid rgb(54, 8, 219);
    border-radius: 0px 15px 0px 15px ;
  }
  .todos-item span{
    width: 80%;
  }
  .todos-item i{
    font-size: 1rem;
    color: rgb(153, 153, 153);
    cursor: pointer;
  }
  .todos-item input[type="checkbox"]{
    display: none;
  }
  .todos-item label{
    margin: 0;
    cursor: pointer;
    color: #fff;
    width: 25px;
    height: 25px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .completed{
    background-color: rgb(0, 201, 0);
  }
  .no-completed{
    background-color: rgb(153, 153, 153);
  }
</style>