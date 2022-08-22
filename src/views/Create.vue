<script setup>
import { reactive, ref, onBeforeMount } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
import FormInput from "../components/FormInput.vue";
import FormTextarea from "../components/FormTextarea.vue";

const router = useRouter();

const books = reactive({
  judul: "",
  isbn: "",
  penerbit: "",
  penulis: "",
  deskripsi: "",
});

const token = ref(sessionStorage.getItem("token"));

const createBook = async () => {
  try {
    const response = await axios.post(
      "http://159.223.57.121:8080/books",
      {
        author: books.penulis,
        description: books.deskripsi,
        isbn: books.isbn,
        publisher: books.penerbit,
        title: books.judul,
      },
      {
        headers: {
          Authorization: `${token.value}`,
        },
      }
    );
    alert(response.data.message);
    router.push({
      name: "Dashboard",
    });
  } catch (error) {
    console.log(error);
  }
};

onBeforeMount(() => {
  document.title = "Create Book";
});
</script>

<template>
  <div class="w-full h-screen flex items-center justify-center">
    <div class="shadow-lg rounded p-4 flex flex-col items-center">
      <h5 class="text-xl font-semibold mb-4">Tambah Buku</h5>
      <hr class="w-full mb-4" />
      <form class="flex flex-col gap-4" @submit.prevent="createBook">
        <FormInput v-model="books.judul">Judul</FormInput>
        <FormInput v-model="books.isbn">ISBN</FormInput>
        <FormInput v-model="books.penerbit">Penerbit</FormInput>
        <FormInput v-model="books.penulis">Penulis</FormInput>
        <FormTextarea v-model="books.deskripsi">Deskripsi</FormTextarea>
        <div class="flex items-center w-full gap-8">
          <RouterLink
            to="/dashboard"
            class="border-2 border-blue-500 text-blue-500 w-full text-center inline-block py-2.5"
            >Kembali</RouterLink
          >
          <button
            type="submit"
            class="bg-blue-500 w-full inline-block py-2.5 text-white"
          >
            Tambah
          </button>
        </div>
      </form>
    </div>
  </div>
</template>
