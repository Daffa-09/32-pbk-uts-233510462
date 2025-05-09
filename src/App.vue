<template>
  <div class="container">
    <h1>Daftar Kegiatan</h1>

    <form @submit.prevent="tambahKegiatan">
      <input v-model="kegiatanBaru" placeholder="Nama kegiatan" required />
      <button type="submit">Tambah</button>
    </form>

    <ul>
      <li v-for="(kegiatan, index) in daftarKegiatan" :key="index">
        <input type="checkbox" v-model="kegiatan.selesai" />
        <span>{{ kegiatan.nama }}</span>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const kegiatanBaru = ref('')
const daftarKegiatan = ref([])

const tambahKegiatan = () => {
  if (kegiatanBaru.value.trim()) {
    daftarKegiatan.value.push({
      nama: kegiatanBaru.value,
      selesai: false,
    })
    kegiatanBaru.value = ''
  }
}
</script>

<style scoped>
.container {
  max-width: 500px;
  margin: 2rem auto;
  padding: 1.5rem;
  background-color: #fff;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  color: #000; /* Warna default hitam */
}

h1 {
  text-align: center;
  margin-bottom: 1.25rem;
  color: #000;
}

form {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

input[type="text"] {
  flex: 1;
  padding: 0.5rem;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  color: #000;
}

button {
  padding: 0.5rem 1rem;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

ul {
  list-style: none;
  padding: 0;
  color: #000;
}

li {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.75rem;
}

span {
  color: #000;
}
</style>
