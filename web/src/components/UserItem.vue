<template>
  <div
    class="bg-white shadow rounded-xl hover:scale-105 transition-transform duration-300 ease-in-out"
  >
    <router-link :to="{ name: 'userDetails', params: { id: user.id } }">
      <img
        :src="'https://kaynakbaltas.com/wp-content/uploads/2021/12/calisan-degil-butun-bir-insan-2.jpg'"
        :alt="`${user.name} ${user.surname}`"
        class="rounded-t-xl w-full h-48 object-cover"
      />
    </router-link>
    <div class="p-4">
      <h3 class="font-bold text-xl text-gray-900">
        {{ user.name }} {{ user.surname }}
      </h3>
      <p class="text-gray-600 mb-2">{{ user.email }}</p>
      <p class="text-gray-600">{{ user.company.name }}</p>
    </div>
  </div>
</template>

<script setup>
const { user } = defineProps({
  user: {
    required: true,
    type: Object,
  },
});
</script>

<style scoped>
.bg-white {
  background-color: #ffffff;
}

.shadow {
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.rounded-xl {
  border-radius: 1rem;
}

.transition-transform {
  transition-property: transform;
}

.duration-300 {
  transition-duration: 300ms;
}

.ease-in-out {
  transition-timing-function: ease-in-out;
}

.hover\:scale-105:hover {
  transform: scale(1.05);
}

.w-full {
  width: 100%;
}

.h-48 {
  height: 12rem;
}

.object-cover {
  object-fit: cover;
}

.p-4 {
  padding: 1rem;
}

.font-bold {
  font-weight: 700;
}

.text-xl {
  font-size: 1.25rem;
}

.text-gray-900 {
  color: #1a202c;
}

.text-gray-600 {
  color: #718096;
}

.mb-2 {
  margin-bottom: 0.5rem;
}
</style>
