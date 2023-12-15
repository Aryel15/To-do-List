<script setup>
    import { toRefs } from 'vue';
    const emit = defineEmits(['close'])
    const props = defineProps({
        variant: String,
        text: String
    });
    const { variant, text } = toRefs(props)

    function onClick(){
        emit('close')
    }
</script>

<template>
    <div :class="['alert', variant ? `alert-${variant}` : '']">
        {{ text }}
        <button @click="onClick">x</button>
    </div>
</template>

<style scoped>
    .alert{
        min-width: 300px;
        padding: 5px 10px;
        border-radius: 5px;
        color: gray;   
        background-color:rgb(241, 241, 241); 
        margin: 10px;
        border-bottom: 4px gray solid;
        display: flex;
        justify-content: space-between;
    }
    .alert button{
        color: #42b983;
        cursor: pointer;
        border: none;
    }
    .alert-success{
        border-bottom: 4px solid #42b983;
        color: #42b983;
    }
</style>