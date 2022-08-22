<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const username = ref(sessionStorage.getItem("profile"));
const isDropdown = ref(false);
const logoutHandler = () => {
  sessionStorage.removeItem("profile");
  sessionStorage.removeItem("token");
  router.push({
    name: "Login",
  });
};
</script>

<template>
  <header class="py-4">
    <nav class="container mx-auto flex items-center justify-between">
      <h1 class="text-3xl font-semibold text-center text-black-800 ml-16">LOGO</h1>
      <button
        @click="isDropdown = !isDropdown"
        class="flex relative items-center mr-8"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          width="24"
          height="24"
        >
          <path fill-rule="evenodd"/>
          <path
            d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z"
          />
        </svg>
        <div
          v-if="isDropdown"
          class="text-white absolute right-0 py-5 px-12 top-8 bg-slate-600 shadow-md"
        >
          <button @click="loginHandler">Change Password</button>
          <button @click="logoutHandler">Logout</button>
          
        </div>
      </button>
    </nav>
  </header>
</template>
