<script setup>
import { ref, onMounted, computed, watch } from "vue";
// ref - how state is managed
// onMounted - allows initial render of page
//mcomputed - arranges data by a certain order
// watch - listens for any changes in ref/state, and updates the ref/state

const todos = ref([]);
const name = ref("");

const input_content = ref("");
const input_catagory = ref("");

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

//Allows the value name to be updated with a new item which is stored in local storage
watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

//Allows for the value stord in local storage to be retrieved
onMounted(() => {
  localStorage.setItem("name", newVal);
});

const addTodo = () => {};
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
        {{ input_content }}
      </form>
    </section>
  </main>
</template>
