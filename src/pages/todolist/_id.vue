<template>
    <div>Modify Page</div>
    <div class="mb-2">
        <label>상태</label>
        <div>
            <button 
                class="btn"
                type="button"
                :class="beforeTodo.completed ? 'btn-success' : 'btn-danger' "
                @click="toggleTodoStatus"
                >
                {{ beforeTodo.completed ? 'Completed' : 'Incomplete'}}
            </button>
        </div>
    </div>
    <form @submit.prevent = "onUpdate">
        <div class="form-group">
            <label>내용</label>
            <textarea v-model="beforeTodo.content" class="form-control" row="3"></textarea>
        </div>
        <div class = "mt-2">
            <button type="submit" class="btn btn-primary">저장</button>
            <button type="button" class="btn btn-outlune-dark ms-2" @click="moveTotodoListPage">취소</button>
        </div>
    </form>
</template>

<script>
import axios from 'axios';
import { useRoute, useRouter } from "vue-router";
import { ref } from 'vue';

export default {
    setup() {
        const route = useRoute();
        const router = useRouter();
        const beforeTodo = ref({});

        async function getTodo() {
            const res = await axios.get("http://localhost:3000/todoList/" + route.params.id);
            beforeTodo.value = res.data;
        }
        
        function toggleTodoStatus(){
            beforeTodo.value.completed = !beforeTodo.value.completed;
        }

        getTodo();

        function moveTotodoListPage() {
            router.push({
                name: 'TodoList'
            })
        }

        async function onUpdate() {
            const res = await axios.patch("http://localhost:3000/todolist/" + route.params.id, {
                content: beforeTodo.value.content,
                completed: beforeTodo.value.completed,
            });
            console.log(res);
        }
        return { beforeTodo, toggleTodoStatus, moveTotodoListPage, onUpdate };
    },     
};
</script>

<style lang="scss" scoped>

</style>