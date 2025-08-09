<script setup>
import { onMounted, ref } from "vue";

let EditIndex = null;
const Todo = ref("");
const Todos = ref([]);

onMounted(() => {
  const saved = localStorage.getItem("Todos");
  if (saved) {
    Todos.value = JSON.parse(saved);
  }
});

function SaveToLocalStorage() {
  localStorage.setItem("Todos", JSON.stringify(Todos.value));
}

function AddToCart() {
  if (Todo.value) {
    if (EditIndex !== null) {
      Todos.value[EditIndex].text = Todo.value;
      EditIndex = null;
      document.getElementById("Todobtn").innerHTML = "Add";
    } else {
      Todos.value.push({ text: Todo.value, done: false });
    }
    Todo.value = "";
  }
  SaveToLocalStorage();
}

function DoneTask(index) {
  Todos.value[index].done = !Todos.value[index].done;
  SaveToLocalStorage();
}

function DeleteTodo(index, event) {
  event.stopPropagation();
  Todos.value.splice(index, 1);
  Todo.value = "";
  SaveToLocalStorage();
}

function EditTodo(index, event) {
  event.stopPropagation();
  document.getElementById("Todobtn").innerHTML = "Edit";
  Todo.value = Todos.value[index].text;
  EditIndex = index;
  SaveToLocalStorage();
}
</script>

<template>
  <div class="flex justify-center items-center min-h-screen" style=" background-image: linear-gradient(to left bottom, #f3b99a, #efa178, #e98957, #e27036, #d9550f);">
    <div
      @keydown.enter="AddToCart"
      class="p-4 border-[1px] bg-white border-gray-200 rounded-xl w-full sm:w-1/2 flex justify-center items-center flex-col"
    >
      <h1 class="font-semibold text-gray-800 text-2xl dancing-script-title">
        Alireza
        <span
          class="font-bold dancing-script-title"
          style="color: #e37b44;"
          >Todo</span>
      </h1>

      <div class="relative w-full">
        <input
          v-model="Todo"
          type="text"
          placeholder="Add Todo..."
          class="border-1 border-gray-400 rounded-sm p-2 mt-10 w-full focus:outline-0 text-gray-700"
        />
        <button
          id="Todobtn"
          @click="AddToCart"
          class="absolute top-16 right-10 bg-amber-500 px-4 py-1 rounded-lg text-white cursor-pointer"
        >
          Add
        </button>
      </div>

      <!-- list of Todo -->
      <ul class="w-full mt-8">
        <li
          v-for="(Todo, index) in Todos"
          :key="index"
          @click="DoneTask(index)"
          :class="
            Todo.done
              ? 'line-through text-gray-400 my-3 p-2 rounded-sm'
              : 'my-3 rounded-sm p-2 text-gray-500'
          "
          class="w-full shadow-lg flex justify-between"
          style="border: 1px solid #e37b44"
        >
          {{ Todo.text }}

          <div class="flex gap-2">
            <button class="Edit cursor-pointer" @click="EditTodo(index, $event)">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="#000000"
                stroke-width="1.25"
                stroke-linecap="round"
                stroke-linejoin="round"
                class="lucide lucide-square-pen-icon lucide-square-pen"
              >
                <path
                  d="M12 3H5a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"
                />
                <path
                  d="M18.375 2.625a1 1 0 0 1 3 3l-9.013 9.014a2 2 0 0 1-.853.505l-2.873.84a.5.5 0 0 1-.62-.62l.84-2.873a2 2 0 0 1 .506-.852z"
                />
              </svg>
            </button>
            <button class="Delete cursor-pointer" @click="DeleteTodo(index, $event)">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
                class="lucide lucide-trash2-icon lucide-trash-2 text-red-500"
              >
                <path d="M10 11v6" />
                <path d="M14 11v6" />
                <path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6" />
                <path d="M3 6h18" />
                <path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2" />
              </svg>
            </button>
          </div>
        </li>
      </ul>

      <h6 class="mt-5 text-gray-500">
        Created with
        <span class="text-green-500"
          >Vue.js
          <svg
            class="logo inline"
            viewBox="0 0 128 128"
            width="24"
            height="24"
            data-v-a5664777=""
          >
            <path
              fill="#42b883"
              d="M78.8,10L64,35.4L49.2,10H0l64,110l64-110C128,10,78.8,10,78.8,10z"
              data-v-a5664777=""
            ></path>
            <path
              fill="#35495e"
              d="M78.8,10L64,35.4L49.2,10H25.6L64,76l38.4-66H78.8z"
              data-v-a5664777=""
            ></path></svg
        ></span>
      </h6>
    </div>
  </div>
</template>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Dancing+Script:wght@400..700&display=swap');
  .dancing-script-title {
  font-family: "Dancing Script", cursive;
  font-optical-sizing: auto;
  font-weight: 900;
  font-style: normal;
}
</style>
