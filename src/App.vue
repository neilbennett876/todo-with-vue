<script setup>
import { ref, onMounted, computed, watch } from "vue";
// ref - how state is managed
// onMounted - allows initial render of page
//mcomputed - arranges data by a certain order
// watch - listens for any changes in ref/state, and updates the ref/state

const todos = ref([]);
const name = ref("");

const input_content = ref("");
const input_catagory = ref(null);

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

const addTodo = () => {
  if (input_content.value.trim() === "" || input_catagory.value === null) {
    return;
    //If no todos exist or if there is no catagory selected, no todos will be added
  }
  todos.value.push({
    content: input_content.value,
    catagory: input_catagory.value,
    done: false,
    createdAt: new Date().getTime(),
  });
  input_content.value = "";
  input_catagory.value = null;
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

//Allows todos to be stored on local storage
watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

//Allows the value name to be updated with a new item which is stored in local storage
watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

//Allows for the value stord in local storage to be retrieved
onMounted(() => {
  localStorage.getItem("name" || newVal);
  todos.value = JSON.parse(localStorage.getItem("todos"));
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" placeholder="name" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>Create a Todo!</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input
          type="text"
          class="text"
          placeholder="e.g. Take out the trash"
          v-model="input_content"
        />

        <h4>Pick a catagory</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="catagory"
              value="business"
              v-model="input_catagory"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input
              type="radio"
              name="catagory"
              value="personal"
              v-model="input_catagory"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add todo" />
      </form>
    </section>

    <section class="todo-list">
      <h3>ToDo List!</h3>
      <div class="list">
        <div
          v-for="todo in todos_asc"
          :class="`todo-item ${todo.done && 'done'}`"
        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.catagory}`"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>
          <div class="action">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>
