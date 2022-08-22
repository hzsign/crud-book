<script setup>
import { onBeforeMount, reactive } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
import FormInput from "../components/FormInput.vue";
import FormTextarea from "../components/FormTextarea.vue";

const router = useRouter();

const register = reactive({
  username: "",
  password: "",
  profileName: "",
  address: "",
});

const registerHandler = async () => {
  try {
    const response = await axios.post(
      "http://159.223.57.121:8080/auth/do-register",
      {
        address: register.address,
        password: register.password,
        profileName: register.profileName,
        username: register.username,
      }
    );
    alert(response.data.message);
    router.push({
      name: "Login",
    });
  } catch (error) {
    console.log(error);
  }
};

onBeforeMount(() => {
  document.title = "Register";
});
</script>

<template>
  <div class="w-full h-screen flex items-center justify-center">
    <div class="shadow-lg rounded p-6 flex flex-col items-center">
      <h5 class="text-xl font-semibold mb-2">Register</h5>
      <hr class="w-full mb-2" />
      <form class="flex flex-col gap-4" @submit.prevent="registerHandler">
        <FormInput v-model="register.username">Username</FormInput>
        <FormInput v-model="register.password" type="password"
          >Password</FormInput
        >
        <FormInput v-model="register.profileName">Profile Name</FormInput>
        <FormTextarea v-model="register.address">Address</FormTextarea>
        <button type="submit" class="bg-blue-500 py-2.5 text-white mb-2">
          Sign Up
        </button>
      </form>
    </div>
  </div>
</template>
