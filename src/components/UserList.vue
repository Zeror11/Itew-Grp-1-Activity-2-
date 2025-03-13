<template>
  <div class="container mt-5">
    <div class="card shadow-lg p-4 border-dark rounded">
      <h2 class="text-center text-white mb-4">Registered Users</h2>

      <!-- Users List -->
      <ul class="list-group mb-4">
        <li
          class="list-group-item list-group-item-action"
          v-for="user in users"
          :key="user.id"
        >
          <strong>{{ user.id }}.</strong>
          <strong class="text-white">{{ user.name }}</strong> - 
          <strong class="text-white">({{ user.username }})</strong> - 
          <span class="text-white">{{ user.address}}</span>
          <span class="text-white">{{ user.phone}}</span>
          <span class="text-white">{{ user.email }}</span>
        </li>
      </ul>

      <!-- Back Button -->
      <button
        class="btn btn-outline-light w-100"
        @click="$emit('back-to-registration')"
      >
        Back to Registration
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return { users: [] };
  },
  methods: {
    async fetchUsers() {
      try {
        const response = await axios.get("https://jsonplaceholder.typicode.com/users");
        this.users = response.data;
      } catch (error) {
        console.error("There was an error!", error);
      }
    },
  },
  mounted() {
    this.fetchUsers();
  },
};
</script>

<style scoped>
/* Custom Styling */
.card {
  max-width: 700px;
  margin: 0 auto;
  padding: 2rem;
  background-color: #333; /* Dark background for card */
  color: white; /* White text */
}

.list-group-item {
  background-color: #444; /* Dark background for list items */
  border: 1px solid #555; /* Slightly lighter border */
  color: white; /* White text for list items */
}

.list-group-item:hover {
  background-color: #555; /* Lighter dark background on hover */
}

.list-group-item .text-white {
  color: white !important; /* Ensure text remains white */
}

.btn-outline-light {
  background-color: transparent;
  color: white;
  border: 1px solid #fff;
}

.btn-outline-light:hover {
  background-color: #555; /* Light dark background on hover */
  color: white;
  border-color: #fff;
}

@media (max-width: 576px) {
  .container {
    padding-left: 15px;
    padding-right: 15px;
  }
}
</style>
