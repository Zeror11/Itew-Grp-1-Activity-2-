<template>
  <div class="container mt-5">
    <div class="card shadow-lg p-5 border-dark rounded">
      <h2 class="text-center text-white mb-4">Register</h2>
      <form @submit.prevent="registerUser">
        <!-- Name Input -->
        <div class="mb-3">
          <label for="name" class="form-label text-white">Name:</label>
          <input
            type="text"
            id="name"
            class="form-control"
            v-model="user.name"
            :class="{'is-invalid': errors.name}"
            placeholder="Enter your name"
          />
          <div v-if="errors.name" class="invalid-feedback">{{ errors.name }}</div>
        </div>

        <!-- Email Input -->
        <div class="mb-3">
          <label for="email" class="form-label text-white">Email:</label>
          <input
            type="email"
            id="email"
            class="form-control"
            v-model="user.email"
            :class="{'is-invalid': errors.email}"
            placeholder="Enter your email"
          />
          <div v-if="errors.email" class="invalid-feedback">{{ errors.email }}</div>
        </div>

        <!-- Password Input -->
        <div class="mb-3">
          <label for="password" class="form-label text-white">Password:</label>
          <input
            type="password"
            id="password"
            class="form-control"
            v-model="user.password"
            :class="{'is-invalid': errors.password}"
            placeholder="Enter your password"
          />
          <div v-if="errors.password" class="invalid-feedback">{{ errors.password }}</div>
        </div>

        <!-- Age Input -->
        <div class="mb-3">
          <label for="age" class="form-label text-white">Age:</label>
          <input
            type="number"
            id="age"
            class="form-control"
            v-model.number="user.age"
            :class="{'is-invalid': errors.age}"
            placeholder="Enter your age"
          />
          <div v-if="errors.age" class="invalid-feedback">{{ errors.age }}</div>
        </div>

        <!-- Submit Button -->
        <button type="submit" class="btn btn-dark w-100 mt-3">
          Register
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      user: { name: "", email: "", password: "", age: null },
      errors: { name: "", email: "", password: "", age: "" },
    };
  },
  methods: {
    validateForm() {
      this.errors = { name: "", email: "", password: "", age: "" };
      let isValid = true;

      if (!this.user.name.trim()) {
        this.errors.name = "Name is required.";
        isValid = false;
      } else if (this.user.name.length < 3) {
        this.errors.name = "Name must be at least 3 characters.";
        isValid = false;
      }

      if (!this.user.email.trim()) {
        this.errors.email = "Email is required.";
        isValid = false;
      } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(this.user.email)) {
        this.errors.email = "Invalid email format.";
        isValid = false;
      }

      if (!this.user.password.trim()) {
        this.errors.password = "Password is required.";
        isValid = false;
      } else if (this.user.password.length < 6) {
        this.errors.password = "Password must be at least 6 characters.";
        isValid = false;
      }

      if (this.user.age === null || this.user.age === "") {
        this.errors.age = "Age is required.";
        isValid = false;
      } else if (!Number.isInteger(this.user.age)) {
        this.errors.age = "Age must be a valid integer.";
        isValid = false;
      } else if (this.user.age <= 18) {
        this.errors.age = "Age must be greater than 18.";
        isValid = false;
      }

      return isValid;
    },
    async registerUser() {
      if (this.validateForm()) {
        try {
          const response = await axios.post(
            "https://jsonplaceholder.typicode.com/users",
            this.user
          );
          if (response.status === 201) {
            this.$emit("user-registered");
          }
        } catch (error) {
          console.error("There was an error!", error);
        }
      }
    },
  },
};
</script>

<style scoped>
/* Add custom styles for the form */
.card {
  max-width: 500px;
  margin: 0 auto;
  padding: 2rem;
  background-color: #333; /* Dark background */
  color: white; /* White text */
}

.form-control {
  background-color: white; /* White background for inputs */
  border: 1px solid #444; /* Dark border */
  color: #333; /* Dark text for better readability */
}

.form-control:focus {
  background-color: #f8f9fa; /* Lighter white background on focus */
  border-color: #666; /* Lighter border on focus */
}

.btn-dark {
  background-color: #000; /* Black button */
  border-color: #333; /* Dark border */
  color: white; /* White text */
}

.btn-dark:hover {
  background-color: #444; /* Lighter black on hover */
  border-color: #555; /* Slightly lighter border on hover */
}

.invalid-feedback {
  display: block;
  color: #ff5555; /* Red color for errors */
}

/* Responsive Design */
@media (max-width: 576px) {
  .container {
    padding-left: 15px;
    padding-right: 15px;
  }
}
</style>
