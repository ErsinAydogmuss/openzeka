<template>
  <div class="p-8 pb-4 text-blue-500 text-center">
    <h1 class="text-3xl font-bold mb-4">Users</h1>
  </div>
  <Users :users="users" />
</template>

<script setup>
import { computed, onMounted, ref } from "vue";
import store from "../store";
import Users from "../components/Users.vue";
import axiosClient from "../axiosClient.js";

const users = ref([]);

onMounted(async () => {
  axiosClient
    .get(`https://jsonplaceholder.typicode.com/users`)
    .then(({ data }) => {
      users.value = data;
    });
});
</script>
