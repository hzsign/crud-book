<script setup>
import TheHeader from "../components/TheHeader.vue";
import axios from "axios";
import { onBeforeMount, onMounted, ref } from "vue";

const token = ref(sessionStorage.getItem("token"));
const datas = ref([]);
const loading = ref(true);

const getData = async () => {
  try {
    const response = await axios.get(
      "http://159.223.57.121:8080/books?limit=20&offset=0",
      {
        headers: {
          authorization: `${token.value}`,
        },
      }
    );
    const { data } = response.data;
    datas.value = data;
    loading.value = false;
  } catch (error) {
    console.log(error);
  }
};

const deleteData = async (id) => {
  try {
    const response = await axios.delete(
      `http://159.223.57.121:8080/books/${id}`,
      {
        headers: {
          authorization: `${token.value}`,
        },
      }
    );
    alert(response.data.message);
    getData();
  } catch (error) {
    console.log(error);
  }
};

onMounted(() => {
  getData();
});

onBeforeMount(() => {
  document.title = "Dashboard";
});
</script>

<template>
  <TheHeader />
  <main class="flex">
  <div class="flex flex-col justify-between w-1/6 h-screen bg-slate-600">
    <ul class="relative">
      <li class="relative">
        <a class="flex items-center text-sm py-4 px-6 h-12 overflow-hidden text-white text-ellipsis whitespace-nowrap rounded hover:text-gray-900 hover:bg-gray-100 transition duration-300 ease-in-out" href="#!" data-mdb-ripple="true" data-mdb-ripple-color="dark">Katalog Buku</a>
      </li>
      <li class="relative">
        <a class="flex items-center text-sm py-4 px-6 h-12 overflow-hidden text-white text-ellipsis whitespace-nowrap rounded hover:text-gray-900 hover:bg-gray-100 transition duration-300 ease-in-out" href="#!" data-mdb-ripple="true" data-mdb-ripple-color="dark">Peminjaman</a>
      </li>
    </ul>
  </div>
  <div class="flex-col justify-between w-5/6 mx-10">
    <div class="flex justify-between mb-8">
    <RouterLink to="/book/create"
    class="bg-blue-600 inline-block text-white flex-initial mr-26 px-8 py-4">Tambah</RouterLink>
    <div></div>
    <div class="flex border-2 rounded">
        <input type="text" class="px-4 py-2 w-80" placeholder="Search">
        <button class="flex items-center justify-center px-3 border-l">
            <svg class="w-6 h-6 text-gray-600" fill="currentColor" xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24">
                <path
                    d="M16.32 14.9l5.39 5.4a1 1 0 0 1-1.42 1.4l-5.38-5.38a8 8 0 1 1 1.41-1.41zM10 16a6 6 0 1 0 0-12 6 6 0 0 0 0 12z" />
            </svg>
        </button>
    </div>
  </div>
  <div v-if="loading">Loading...</div>
    <table v-else class="w-full border-collapse border border-black">
      <thead>
        <tr>
          <th class="border border-black">No</th>
          <th class="border border-black">Judul</th>
          <th class="border border-black">ISBN</th>
          <th class="border border-black">Penerbit</th>
          <th class="border border-black">Deskripsi</th>
          <th class="border border-black">Aksi</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="data in datas" :key="data.id">
          <td class="border border-black text-center">
            {{ data.id }}
          </td>
          <td class="border border-black p-1">
            {{ data.title }}
          </td>
          <td class="border border-black p-1">
            {{ data.isbn }}
          </td>
          <td class="border border-black p-1">
            {{ data.publisher }}
          </td>
          <td class="border border-black p-1">
            {{ data.description }}
          </td>
          <td class="border border-black p-1">
            <RouterLink
              :to="{ name: 'Edit', params: { id: data.id } }"
              class="w-1/2 text-center inline-block"
              >Edit</RouterLink
            >
            <button
              @click.prevent="deleteData(data.id)"
              class="text-red-400"
            >
              Hapus
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  </main>
</template>
