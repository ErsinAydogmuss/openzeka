<template>
  <div class="container mx-auto p-6 flex">
    <nav class="navbar w-1/4 p-4 bg-gray-100 rounded-lg shadow-lg mr-6">
      <ul>
        <li
          v-for="section in sections"
          :key="section"
          @click="setActiveSection(section)"
          class="cursor-pointer p-2 hover:bg-gray-200 rounded flex items-center"
          :class="{ 'bg-blue-500 text-white': activeSection === section }"
        >
          <i
            :class="icons[section]"
            class="mr-2 icon"
            :style="{ fontSize: '1.5rem', color: iconColors[section] }"
          ></i>

          <span class="menu-text">{{ section }}</span>
        </li>
      </ul>
    </nav>

    <div class="content w-3/4">
      <div v-if="loading" class="text-center text-2xl text-gray-500">
        Loading...
      </div>
      <div v-else>
        <div
          v-if="activeSection === 'User Details'"
          class="user-card rounded-lg shadow-lg p-8"
        >
          <h1 class="text-3xl font-semibold mb-6 text-center text-blue-700">
            {{ user.name }}
          </h1>
          <img
            :src="'https://kaynakbaltas.com/wp-content/uploads/2021/12/calisan-degil-butun-bir-insan-2.jpg'"
            :alt="user.name"
            class="profile-image mb-6 rounded-lg shadow-md"
          />
          <div
            class="grid grid-cols-1 gap-6 text-base sm:grid-cols-2 md:grid-cols-3"
          >
            <div class="info-card bg-gray-100 p-4 rounded-lg shadow-md">
              <strong class="block font-bold text-gray-700">Username:</strong>
              <p>{{ user.username }}</p>
            </div>
            <div class="info-card bg-gray-100 p-4 rounded-lg shadow-md">
              <strong class="block font-bold text-gray-700">Email:</strong>
              <p>{{ user.email }}</p>
            </div>
            <div class="info-card bg-gray-100 p-4 rounded-lg shadow-md">
              <strong class="block font-bold text-gray-700">Phone:</strong>
              <p>{{ user.phone }}</p>
            </div>
            <div class="info-card bg-gray-100 p-4 rounded-lg shadow-md">
              <strong class="block font-bold text-gray-700">Website:</strong>
              <a
                :href="'http://' + user.website"
                class="text-blue-600 underline"
                target="_blank"
              >
                {{ user.website }}
              </a>
            </div>
            <div class="info-card bg-gray-100 p-4 rounded-lg shadow-md">
              <strong class="block font-bold text-gray-700">Address:</strong>
              <p>{{ user.address.street }}, {{ user.address.suite }}</p>
              <p>{{ user.address.city }}, {{ user.address.zipcode }}</p>
            </div>
            <div class="info-card bg-gray-100 p-4 rounded-lg shadow-md">
              <strong class="block font-bold text-gray-700">Company:</strong>
              <p>{{ user.company.name }}</p>
              <p>{{ user.company.catchPhrase }}</p>
            </div>
          </div>
        </div>

        <div v-else class="section-content">
          <h2 class="text-2xl font-semibold mb-4">{{ activeSection }}</h2>
          <div v-if="sectionLoading" class="text-center text-2xl text-gray-500">
            Loading...
          </div>
          <div v-else>
            <ul>
              <li v-for="item in sectionData" :key="item.id" class="mb-4">
                <div
                  v-if="activeSection === 'Posts'"
                  class="post-card bg-white p-4 rounded-lg shadow-md"
                >
                  <h3 class="font-bold text-lg text-blue-700">
                    {{ item.title }}
                  </h3>
                  <p>{{ item.body }}</p>
                  <p
                    v-if="item.date"
                    class="text-gray-500 text-sm text-right mt-2"
                  >
                    {{ item.date }}
                  </p>
                </div>

                <div
                  v-if="activeSection === 'Comments'"
                  class="comment-card bg-white p-4 rounded-lg shadow-md"
                >
                  <h3 class="font-bold text-lg text-blue-700">
                    {{ item.name }}
                  </h3>
                  <p class="text-gray-600">{{ item.email }}</p>
                  <p>{{ item.body }}</p>
                  <p
                    v-if="item.date"
                    class="text-gray-500 text-sm text-right mt-2"
                  >
                    {{ item.date }}
                  </p>
                </div>

                <div
                  v-if="activeSection === 'Albums'"
                  class="album-card bg-white p-4 rounded-lg shadow-md flex items-center"
                >
                  <i class="fas fa-music mr-2"></i>
                  <h3 class="font-bold text-lg text-blue-700">
                    {{ item.title }}
                  </h3>
                </div>

                <div
                  v-if="activeSection === 'Photos'"
                  class="photo-card bg-white p-4 rounded-lg shadow-md flex items-center"
                >
                  <img
                    :src="item.thumbnailUrl"
                    :alt="item.title"
                    class="rounded-lg shadow-md w-40 h-40 mr-4"
                  />
                  <div>
                    <h3 class="font-bold text-lg text-blue-700">
                      {{ item.title }}
                    </h3>
                  </div>
                </div>

                <div
                  v-if="activeSection === 'Todos'"
                  class="todo-card bg-white p-4 rounded-lg shadow-md flex items-center"
                >
                  <input
                    type="checkbox"
                    :checked="item.completed"
                    class="mr-4 w-6 h-6"
                    disabled
                  />
                  <p
                    :class="{ 'line-through': item.completed }"
                    class="text-lg"
                  >
                    {{ item.title }}
                  </p>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, onMounted, ref, watch } from "vue";
import { useRoute } from "vue-router";
import axiosClient from "../axiosClient";

const route = useRoute();
const user = ref({});
const loading = ref(true);

const sections = [
  "User Details",
  "Posts",
  "Comments",
  "Albums",
  "Photos",
  "Todos",
];

const icons = {
  "User Details": "fas fa-user",
  Posts: "fas fa-file-alt",
  Comments: "fas fa-comments",
  Albums: "fas fa-music",
  Photos: "fas fa-camera",
  Todos: "fas fa-check-square",
};

const iconColors = {
  "User Details": "#3182ce",
  Posts: "#38a169",
  Comments: "#f6ad55",
  Albums: "#fc8181",
  Photos: "#e53e3e",
  Todos: "#d69e2e",
};

const iconColor = computed(() => iconColors[activeSection.value] || "#4a5568");

const activeSection = ref("User Details");
const sectionData = ref([]);
const sectionLoading = ref(false);

const fetchUserDetails = async () => {
  loading.value = true;
  await axiosClient
    .get(`https://jsonplaceholder.typicode.com/users/${route.params.id}`)
    .then(({ data }) => {
      user.value = data || {};
      loading.value = false;
    });
};

const fetchSectionData = async () => {
  sectionLoading.value = true;
  let endpoint = "";
  switch (activeSection.value) {
    case "Posts":
      endpoint = `https://jsonplaceholder.typicode.com/users/${route.params.id}/posts`;
      break;
    case "Comments":
      endpoint = `https://jsonplaceholder.typicode.com/users/${route.params.id}/comments`;
      break;
    case "Albums":
      endpoint = `https://jsonplaceholder.typicode.com/users/${route.params.id}/albums`;
      break;
    case "Photos":
      endpoint = `https://jsonplaceholder.typicode.com/albums/${route.params.id}/photos`;
      break;
    case "Todos":
      endpoint = `https://jsonplaceholder.typicode.com/users/${route.params.id}/todos`;
      break;
  }
  if (endpoint) {
    await axiosClient.get(endpoint).then(({ data }) => {
      sectionData.value = data || [];
      sectionLoading.value = false;
    });
  }
};

const setActiveSection = (section) => {
  activeSection.value = section;
};

watch(activeSection, fetchSectionData);

onMounted(async () => {
  await fetchUserDetails();
});
</script>

<style scoped>
/* Container */
.container {
  max-width: 1200px;
  padding: 2rem;
  margin: 0 auto;
}

/* Navbar */
.navbar {
  background-color: #f5f5f5;
  border-radius: 0.5rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.navbar ul li {
  transition: background-color 0.3s ease;
  display: flex;
  align-items: center;
}

.navbar ul li.bg-blue-500 {
  background-color: #4299e1;
  color: #fff;
}

.navbar ul li.bg-blue-500:hover {
  background-color: #2b6cb0;
}

.navbar ul li i {
  font-size: 1.5rem;
}

.menu-text {
  font-size: 1.1rem; /* Slightly larger font size for better readability */
}

/* User Card */
.user-card {
  background-color: #fff;
  border-radius: 0.5rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 2rem;
}

/* Info Cards */
.info-card {
  background-color: #f5f5f5;
  border-radius: 0.5rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 1rem;
}

/* Section Content */
.section-content {
  background-color: #fff;
  border-radius: 0.5rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 1rem;
}

/* Post, Comment, Album, Photo, Todo Cards */
.post-card,
.comment-card,
.album-card,
.photo-card,
.todo-card {
  background-color: #fff;
  border-radius: 0.5rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 1rem;
  margin-bottom: 1rem;
}

.post-card h3,
.comment-card h3,
.album-card h3,
.photo-card h3 {
  font-size: 1.25rem;
  color: #1a202c;
}

.post-card p,
.comment-card p,
.album-card p,
.photo-card p,
.todo-card p {
  font-size: 1rem;
  color: #4a5568;
}

.photo-card img {
  border-radius: 0.5rem;
  width: 120px;
  height: 120px;
}

.todo-card input[type="checkbox"] {
  width: 1.5rem;
  height: 1.5rem;
}

.line-through {
  text-decoration: line-through;
}

.text-right {
  text-align: right;
}

.text-sm {
  font-size: 0.875rem;
}

.text-lg {
  font-size: 1.125rem;
}

.text-base {
  font-size: 1rem;
}

.text-blue-700 {
  color: #2b6cb0;
}

.text-blue-600 {
  color: #3182ce;
}

.text-gray-500 {
  color: #a0aec0;
}

.text-gray-600 {
  color: #718096;
}

.text-gray-700 {
  color: #4a5568;
}

.text-center {
  text-align: center;
}

.icon {
  font-size: 1.5rem;
}

.navbar .icon-user-details {
  color: #3182ce;
}

.navbar .icon-posts {
  color: #38a169;
}

.navbar .icon-comments {
  color: #f6ad55;
}

.navbar .icon-albums {
  color: #fc8181;
}

.navbar .icon-photos {
  color: #e53e3e;
}

.navbar .icon-todos {
  color: #d69e2e;
}

.navbar ul li.bg-blue-500 .icon {
  color: #fff;
  font-size: 1.25rem;
}
</style>
