<template>
  <header
    class="bg-gradient-to-r from-blue-500 to-indigo-500 shadow-lg h-16 flex justify-between items-center px-6"
  >
    <router-link
      :to="{ name: 'home' }"
      class="inline-flex items-center h-full px-5 text-white font-bold text-lg transition duration-300 ease-in-out transform hover:scale-105"
    >
      Home
    </router-link>
  </header>
</template>

<style scoped>
.bg-gradient-to-r {
  background-image: linear-gradient(to right, #4299e1, #667eea);
}

.shadow-lg {
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
}

.h-16 {
  height: 4rem;
}

.px-6 {
  padding-left: 1.5rem;
  padding-right: 1.5rem;
}

.text-white {
  color: #ffffff;
}

.font-bold {
  font-weight: 700;
}

.font-medium {
  font-weight: 500;
}

.text-lg {
  font-size: 1.125rem;
}

.text-sm {
  font-size: 0.875rem;
}

.transition {
  transition: all 0.3s ease-in-out;
}

.duration-300 {
  transition-duration: 300ms;
}

.ease-in-out {
  transition-timing-function: ease-in-out;
}

.transform {
  transform: scale(1);
}

.hover\:scale-105:hover {
  transform: scale(1.05);
}

.inline-flex {
  display: inline-flex;
  align-items: center;
}

.items-center {
  align-items: center;
}

.justify-between {
  justify-content: space-between;
}

.gap-4 {
  gap: 1rem;
}
</style>
