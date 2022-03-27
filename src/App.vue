<script setup>
import { computed } from '@vue/reactivity';
import { ref } from 'vue';

const message = ref('Hello World')
const count = ref(0)
const text = ref('')
const toggleBool = ref('false')

// function onInput(e) {
//     text.value = e.target.value
// }

function toggle() {
    toggleBool.value = !toggleBool.value
}


let id = 0
const newTodo = ref('')
const hideCompletedBool = ref(false)
const todos = ref([
    {id: id++, task:'Fold clothes', done: false},
    {id: id++, task:'Make bed', done: false},
    {id: id++, task:'Rake leaves', done: false}
])

const filteredTodos = computed(() => {
    return hideCompletedBool.value
    ? todos.value.filter(t => !t.done)
    : todos.value
})

function addTodo() {
    todos.value.push({ id: id++, task: newTodo.value, done: false })
    newTodo.value = ''
}

function removeTodo(todo) {
    todos.value = todos.value.filter(t => t !== todo) // sorts every element that is not todo into a new array
}

function hideCompleted() {
    hideCompletedBool.value = !hideCompletedBool.value
}
</script>

<template>
<div>
    <p>{{message}}</p>
    <button v-on:click="count++">{{ count }}</button>
</div>
<div>
    <!-- <input :value="text" @input="onInput" placeholder="Type here">
    <p>{{ text }}</p> -->
    <input v-model="text" placeholder="Type Here">
    <p>{{ text }}</p>
</div>
<div>
    <button @click="toggle()">Toggle</button>
    <h1 v-if="toggleBool">Vue is cool!</h1>
    <h1 v-else>Off 😢</h1>
</div>
<div>
    <form @submit.prevent="addTodo()">
        <input v-model="newTodo" placeholder="New Task">
        <button>Add Todo</button>
    </form>
    <ul>
        <li v-for="todo in filteredTodos" :key="todo.id">
            <input type="checkbox" v-model="todo.done">
            <span :class="{done: todo.done}">{{ todo.task }}</span>
            <button @click="removeTodo(todo)">X</button>
        </li>
    </ul>
    <button @click="hideCompleted()">
        {{ hideCompletedBool ? 'Show all' : 'Hide completed'}}
    </button>
</div>
</template>

<style>
    .done {
        text-decoration: line-through;
    }
</style>