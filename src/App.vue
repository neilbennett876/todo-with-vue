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
    return a.createdAt - b.createdAt;
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
  </main>
</template>
