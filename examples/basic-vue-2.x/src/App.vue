<script lang="ts">
import { defineComponent } from "@vue/composition-api";
import { useQuery } from "vue-query";
import { VueQueryDevTools } from "vue-query/devtools";

interface Todo {
  userId: number;
  id: number;
  title: string;
  completed: boolean;
}

const todoFetcher = async (): Promise<Todo[]> =>
  await fetch("https://jsonplaceholder.cypress.io/todos").then((response) =>
    response.json()
  );

export default defineComponent({
  components: { VueQueryDevTools },
  setup() {
    const { isLoading, isError, isFetching, data, error, refetch } = useQuery(
      "todos",
      todoFetcher
    );
    useQuery("todos2", todoFetcher);
    useQuery("todos3", todoFetcher);

    return { isLoading, isError, isFetching, data, error, refetch };
  },
});
</script>

<template>
  <div>
    <h1>vue-query example</h1>
    <p>Turn on <b>Slow 3G</b> or <b>Offline</b> in dev-tools and hit Refetch</p>
    <button @click="refetch" :disabled="isFetching">
      {{ isFetching ? "Refetching..." : "Refetch" }}
    </button>
    <h2>TODO list</h2>
    <div v-if="isLoading">Loading...</div>
    <div v-else-if="isError">An error has occurred: {{ error }}</div>
    <div v-else-if="data">
      <ul>
        <li v-for="item in data" :key="item.id">
          {{ item.completed ? "🗹" : "☐" }} {{ item.title }}
        </li>
      </ul>
    </div>
    <div v-else>Nothing to see here...</div>
    <VueQueryDevTools :initialIsOpen="true" />
  </div>
</template>

<style>
ul {
  list-style: none;
}
</style>
