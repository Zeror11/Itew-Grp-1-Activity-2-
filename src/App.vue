<template>
  <div id="app" class="container">
    <!-- Header -->
    <h1 class="text-center text-white bg-dark p-3 rounded mt-4">User Registration System</h1>

    <!-- Success Modal -->
    <div v-if="showSuccessPopup" class="modal fade show d-block" tabindex="-1">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header bg-dark text-white">
            <h5 class="modal-title">Success</h5>
            <button type="button" class="btn-close text-white" @click="closeSuccessPopup"></button>
          </div>
          <div class="modal-body">
            <p>Registration successful! Redirecting to the user list...</p>
          </div>
        </div>
      </div>
    </div>

    <!-- User Registration Component -->
    <transition name="fade" mode="out-in">
      <UserRegistration v-if="!showUsers" @user-registered="handleRegistration" />
    </transition>

    <!-- Loading Spinner -->
    <transition name="fade" mode="out-in">
      <div v-if="isLoading" class="text-center mt-4">
        <div class="spinner-border text-dark" role="status">
          <span class="visually-hidden">Loading...</span>
        </div>
        <p class="mt-2 text-dark">Fetching users...</p>
      </div>
    </transition>

    <!-- User List Component -->
    <transition name="fade" mode="out-in">
      <UserList v-if="showUsers && !isLoading" @back-to-registration="showUsers = false" />
    </transition>
  </div>
</template>

<script>
import UserRegistration from "./components/UserRegistration.vue";
import UserList from "./components/UserList.vue";
import axios from "axios";

export default {
  components: { UserRegistration, UserList },
  data() {
    return {
      showUsers: false,
      isLoading: false,
      showSuccessPopup: false,
    };
  },
  methods: {
    async handleRegistration() {
      this.showSuccessPopup = true;

      setTimeout(() => {
        this.showSuccessPopup = false;
        this.isLoading = true;
        this.showUsers = true;

        setTimeout(async () => {
          try {
            await axios.get("https://jsonplaceholder.typicode.com/users");
          } catch (error) {
            console.error("Error fetching users!", error);
          }
          this.isLoading = false;
        }, 2000);
      }, 2000);
    },
    closeSuccessPopup() {
      this.showSuccessPopup = false;
    },
  },
};
</script>

<style scoped>
/* Smooth fade transition */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.8s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

/* Success Modal */
.modal {
  display: block !important;
  z-index: 1050;
}
.modal-header {
  background-color: #000; /* Black background */
  color: #fff; /* White text */
}
.modal-body {
  font-size: 1rem;
  color: #000; /* Black text */
}

/* Header */
h1 {
  background-color: #000; /* Black background */
  color: #fff; /* White text */
}

/* Loading Spinner */
.spinner-border {
  width: 3rem;
  height: 3rem;
}

/* Custom button for the back to registration */
.btn-outline-dark {
  width: 100%;
  margin-top: 15px;
  border-color: #000;
  color: #000;
}

/* Text and background colors */
.text-dark {
  color: #000; /* Dark text */
}

.bg-dark {
  background-color: #000; /* Dark background */
}

/* User Registration & List Components */
.container {
  margin-top: 50px;
}

/* For smaller screens */
@media (max-width: 576px) {
  .container {
    padding-left: 15px;
    padding-right: 15px;
  }
}
</style>
