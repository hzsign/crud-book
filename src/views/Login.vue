<script setup>
import { onBeforeMount, reactive } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
import FormInput from "../components/FormInput.vue";

const router = useRouter();

const login = reactive({
  username: "",
  password: "",
});

const loginHandler = async () => {
  try {
    const response = await axios.post(
      "http://159.223.57.121:8080/auth/do-login",
      {
        password: login.password,
        username: login.username,
      }
    );
    const { data, message } = response.data;
    if (message === "PASSWORD OR USER NOT REGISTERED") {
      alert(message);
    } else {
      sessionStorage.setItem("token", data.token);
      sessionStorage.setItem("profile", data.username);
      alert(message);
      router.push({
        name: "Dashboard",
      });
    }
  } catch (error) {
    console.log(error);
  }
};

onBeforeMount(() => {
  document.title = "Login";
});
</script>

<template>
  <div class="w-full h-screen flex items-center justify-center">
    <div class="shadow-lg rounded p-6 flex flex-col items-center">
      <h5 class="text-xl font-semibold mb-2">Login</h5>
      <hr class="w-full mb-2" />
      <form class="flex flex-col gap-4" @submit.prevent="loginHandler">
        <FormInput v-model="login.username">Username</FormInput>
        <FormInput type="password" v-model="login.password">Password</FormInput>
        <button type="submit" class="bg-blue-500 py-2.5 text-white mb-2">
          Login
        </button>
      </form>
      <RouterLink to="/" class="mb-5 underline">Forget Password?</RouterLink>
      <hr class="w-full mb-2" />
      <p class="mb-2">New User</p>
      <RouterLink
        to="/register"
        class="bg-blue-500 inline-block w-full text-center py-2.5 text-white"
      >
        Register
      </RouterLink>
    </div>
  </div>
</template>
