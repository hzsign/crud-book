<script setup>
import { onBeforeMount, onMounted, reactive, ref } from "vue";
import { useRouter, useRoute } from "vue-router";
import axios from "axios";
import FormInput from "../components/FormInput.vue";
import FormTextarea from "../components/FormTextarea.vue";

const router = useRouter();
const route = useRoute();

const books = reactive({
  judul: "",
  isbn: "",
  penerbit: "",
  penulis: "",
  deskripsi: "",
});

const token = ref(sessionStorage.getItem("token"));

const editBook = async () => {
  try {
    const response = await axios.put(
      "http://159.223.57.121:8080/books",
      {
        author: books.penulis,
        description: books.deskripsi,
        id: route.params.id,
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

const getData = async () => {
  try {
    const response = await axios.get(
      `http://159.223.57.121:8080/books/findbyid/${route.params.id}`,
      {
        headers: {
          authorization: `${token.value}`,
        },
      }
    );
    const { data } = response.data;
    books.judul = data.title;
    books.isbn = data.isbn;
    books.penerbit = data.publisher;
    books.penulis = data.author;
    books.deskripsi = data.description;
    console.log(data);
  } catch (error) {
    console.log(error);
  }
};

onMounted(() => {
  getData();
});

onBeforeMount(() => {
  document.title = "Edit Book";
});
</script>

<template>
  <div class="w-full h-screen flex items-center justify-center">
    <div class="shadow-lg rounded p-6 flex flex-col items-center">
      <h5 class="text-xl font-semibold mb-2">Edit Buku</h5>
      <hr class="w-full mb-2" />
      <form class="flex flex-col gap-4" @submit.prevent="editBook">
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
            Ubah
          </button>
        </div>
      </form>
    </div>
  </div>
</template>
