  <!-- <div v-show="toggle">true</div> -->
  <!-- <div v-show="!toggle">false</div> -->
  <!-- <button @click="onToggle">Toggle</button> -->
<template>
  <div>
    <!-- <h4>count: {{ count }}</h4>
    <h4>doubleCount: {{ doubleCount }}</h4>
    <h4>doubleCountFunction: {{ doubleCountFunc() }}</h4> -->
    <!-- <button @click="count++">Add one</button> -->
    <h1 class="text-center">게시판✍</h1>
    <input
      class="form-control"
      type="text"
      v-model="searchText"
      placeholder="검색"
      @keyup.enter="searchTodo"
    />
    <hr />
    <SimpleForm @add-todo="addTodo" />
    <div class="text-danger" v-if="axiosErrorMessage.length != 0">
      {{ axiosErrorMessage }}
    </div>
    <div v-show="!todoList.length">추가 된 내용이 없습니다.</div>
    <TodoList
      :todoList="todoList"
      @toggle-todo="toggleTodo"
      @todo-delete="onDelete"

    />
    <hr>
  </div>
  <nav aria-label="Page navigation example">
  <ul class="pagination">
    <li v-if="currentPage !==1" class="page-item">
      <a style="cursor: pointer" class="page-link" @click="getTodoList(currentPage - 1)">Previous</a></li>
    <li
      v-for="loop in numberOfPages"
      :key="loop" class="page-item"
      :class="currentPage === loop ? 'active' : ''"
    >
      <a style="cursor: pointer" class="page-link" @click="getTodoList(loop)">{{ loop }}</a>
    </li>
    <li v-if="numberOfPages !== currentPage" class="page-item">
      <a style="cursor: pointer" class="page-link"  @click="getTodoList(currentPage + 1)">Next</a>
    </li>
  </ul>
  </nav>
</template>

<script>
 import { ref, computed, watch } from "vue";
import SimpleForm from "@/components/SimpleForm.vue";
import TodoList from "@/components/TodoList.vue";
import axios from "axios";

export default {
  components: {
    SimpleForm,
    TodoList,
  },
  setup() {
    const todo = ref("");
    const todoList = ref([]);
    const axiosErrorMessage = ref("");
    const hasError = ref(false);
    const numberOfTodoList = ref(0);
    const currentPage = ref(1);
    const limit = 5;
    const searchText = ref("");

    const numberOfPages = computed(() => {
      return Math.ceil(numberOfTodoList.value / limit);
    });

    let timeout = null;

    const searchTodo = () => {
    clearTimeout(timeout);
    getTodoList(1);
  }

  watch(searchText, () => {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      getTodoList(1);
    }, 2000);
  })

  async function getTodoList(pageNumber = currentPage.value) {
    currentPage.value = pageNumber;
    axiosErrorMessage.value = "";
    try {
    //   let apiUrl = `http://localhost:3000/todolist?_sort=id&_order=desc&_page=${pageNumber}&_limit=${limit}`;

    //   if (searchText.value) {
    //     // Generate an array of initial sounds
    //     const initials = Array.from(searchText.value.trim()).map((char) => char.charCodeAt(0));

    //     // Append the initial sounds to the API URL
    //     apiUrl += `&content_like=${initials.join(",")}`;
    //   }

    //   console.log(apiUrl);

    //   const res = await axios.get(apiUrl);
      const res = await axios.get(`http://localhost:3000/todolist?_sort=id&_order=desc&_page=${pageNumber}&_limit=${limit}&content_like=${searchText.value}`);
      numberOfTodoList.value = res.headers["x-total-count"];
      todoList.value = res.data;
    } catch (err) {
      console.log(err);
      axiosErrorMessage.value = "네트워크 에러가 발생했어요."
    }
  }
    getTodoList();

    async function addTodo(todos) {
      axiosErrorMessage.value="";
      try {
        await axios.post("http://localhost:3000/todoList", {
          content: todos.content,
          completed: todos.completed,
        })
        getTodoList(1);
      } catch(err) {
          axiosErrorMessage.value = "네트워크 에러가 발생했어요."
          console.log(err);
      }
    }

    async function onDelete(index) {
      axiosErrorMessage.value = "";
      const id = todoList.value[index].id;
      try {
        const res = await axios.delete("http://localhost:3000/todoList/" + id);
        console.log(res);
        getTodoList(1);
      } catch (err) {
        axiosErrorMessage.value = "네트워크 에러가 발생했어요.";
        console.log(err);
      }
    }

    async function toggleTodo(index, checked) {
      axiosErrorMessage.value = "";
      const id = todoList.value[index].id;
      try {
        await axios.patch("http://localhost:3000/todoList/" + id, {
          completed: checked
        })
      } catch (err) {
        axiosErrorMessage.value = "네트워크 에러가 발생해서요";
        console.log(err);
      }
      todoList.value[index].completed = checked;
    }


    // const filteredTodoList = computed(() => {
    //   if (searchText.value) {
    //     return todoList.value.filter((loop) => {
    //       return loop.content.includes(searchText.value);
    //     });
    //   }
    //   return todoList.value;
    // });

    return {
      todo,
      todoList,
      /*toggle, onToggle,*/ hasError,
      toggleTodo,
      onDelete,
      addTodo,
      searchText,
      // filteredTodoList,
      axiosErrorMessage,
      numberOfPages,
      currentPage,
      getTodoList,
      searchTodo
    };
  },
};
</script>

<style>
.completed {
  text-decoration: line-through;
  color: gray;
}
</style>