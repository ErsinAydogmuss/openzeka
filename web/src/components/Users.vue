<template>
  <div class="grid grid-cols-1 md:grid-cols-3 gap-5 px-8">
    <UserItem v-for="user of users" :key="user.id" :user="user" />
  </div>
  <div v-if="!users.length" class="flex justify-center text-gray-600 p-8">
    Not Found User!
  </div>
</template>

<script setup>
import UserItem from "./UserItem.vue";

const { users } = defineProps({
  users: {
    required: true,
    type: Array,
  },
});
</script>
