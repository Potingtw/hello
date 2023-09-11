<script>
    import { ref } from 'vue';
    export default {
        name: 'App',
        setup () {
            const newTodo = ref('');
            const defaultData = [{
                done: false,
                content: 'Write a blog post'
            }]
            const todosData = JSON.parse(localStorage.getItem('todos')) ||                         defaultData;
            const todos = ref(todosData);
            function addTodo () {
                if (newTodo.value) {
                    todos.value.push({
                        done: false,
                        content: newTodo.value
                    });
                    newTodo.value = '';
                }
                saveData();
            }
            function doneTodo (todo) {
                todo.done = !todo.done
                saveData();
            }
            function removeTodo (index) {
                todos.value.splice(index, 1);
                saveData();
            }
            function saveData () {
                const storageData = JSON.stringify(todos.value);
                localStorage.setItem('todos', storageData);
            }
            return {
                todos,
                newTodo,
                addTodo,
                doneTodo,
                removeTodo,
                saveData
            }
        }
    }
</script>

<template>
    <h1>ToDoList</h1>
    <form @submit.prevent="addTodo()">
        <label>代辦事項 </label>
        <input
            v-model="newTodo"
            name="newTodo"
            autocomplete="off"
        >
        <button>新增</button>
    </form>
    <h2>未完成事項</h2>
    <ul>
        <li
            v-for="(todo, index) in todos"
            :key="index"
        >
            <span
                :class="{ done: todo.done }"
                @click="doneTodo(todo)"
            >{{ todo.content }}</span>
            <button @click="removeTodo(index)">移除</button>
        </li>
    </ul>
    <h4 v-if="todos.length === 0">Empty list.</h4>
</template>
