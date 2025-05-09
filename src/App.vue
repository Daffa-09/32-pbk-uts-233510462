<template>
  <div class="container">
    <h1>APLIKASI KEGIATAN</h1>

    <form @submit.prevent="addKegiatan">
      <input v-model="kegiatanBaru" placeholder="Nama kegiatan" required />
      <input v-model="waktuBaru" type="time" required />
      <button type="submit">TAMBAH</button>
    </form>

    <div class="filter">
      <label>
        <input type="checkbox" v-model="filterBelumSelesai" />
        TAMPILKAN YANG BELUM SELESAI
      </label>
    </div>

    <ul>
      <li v-for="kegiatan in kegiatanTersaring" :key="kegiatan.id">
        <input type="checkbox" v-model="kegiatan.selesai" />
        <div class="info">
          <span :class="{ selesai: kegiatan.selesai }">{{ kegiatan.nama }}</span>
          <small>{{ formatWaktu(kegiatan.waktu) }}</small>
        </div>
        <div>
          <button class="pengingat-btn" @click="togglePengingat(kegiatan)">
            <span v-if="kegiatan.pengingat">🔔</span>
            <span v-else>🔕</span>
          </button>
          <button class="hapus-btn" @click="hapusKegiatan(kegiatan.id)">HAPUS</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const kegiatanBaru = ref('')
const waktuBaru = ref('')
const daftarKegiatan = ref([])
const filterBelumSelesai = ref(false)

const addKegiatan = () => {
  if (kegiatanBaru.value.trim() && waktuBaru.value) {
    daftarKegiatan.value.push({
      id: Date.now() + Math.random(),
      nama: kegiatanBaru.value,
      waktu: waktuBaru.value,
      selesai: false,
      pengingat: false,
    })
    kegiatanBaru.value = ''
    waktuBaru.value = ''
  }
}

const hapusKegiatan = (id) => {
  daftarKegiatan.value = daftarKegiatan.value.filter(k => k.id !== id)
}

const togglePengingat = (kegiatan) => {
  kegiatan.pengingat = !kegiatan.pengingat
}

const kegiatanTersaring = computed(() => {
  const filtered = filterBelumSelesai.value
    ? daftarKegiatan.value.filter(k => !k.selesai)
    : daftarKegiatan.value

  return [...filtered].sort((a, b) => a.waktu.localeCompare(b.waktu))
})

const formatWaktu = (waktu) => {
  const [jam, menit] = waktu.split(':')
  return `${jam}:${menit}`
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

* {
  box-sizing: border-box;
}

body {
  font-family: 'Poppins', sans-serif;
  background-color: #f2f6f9;
  margin: 0;
  color: #212529;
  text-transform: uppercase;
}

.container {
  max-width: 550px;
  margin: 3rem auto;
  padding: 2rem;
  background-color: #ffffff;
  border-radius: 20px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.07);
}

h1 {
  text-align: center;
  margin-bottom: 1.75rem;
  font-size: 1.8rem;
  color: #2c3e50;
}

form {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
  margin-bottom: 1.2rem;
}

input[type="text"],
input[type="time"] {
  padding: 0.7rem 1rem;
  font-size: 1rem;
  border: 1px solid #d0d7de;
  border-radius: 10px;
  transition: border-color 0.3s;
  color: #212529;
  text-transform: uppercase;
}

input:focus {
  border-color: #00b894;
  outline: none;
}

button {
  padding: 0.6rem 1rem;
  background-color: #00b894;
  color: #ffffff;
  border: none;
  border-radius: 10px;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.3s;
  text-transform: uppercase;
}

button:hover {
  background-color: #019270;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  justify-content: space-between;
  background-color: #f7fafd;
  padding: 0.9rem 1rem;
  border-radius: 12px;
  margin-bottom: 0.8rem;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  transition: background 0.3s;
}

li:hover {
  background-color: #eef4f7;
}

.info {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.info span {
  font-weight: 600;
  font-size: 1rem;
  color: #212529;
  transition: color 0.3s;
  text-transform: uppercase;
}

.info small {
  font-size: 0.85rem;
  color: #6c757d;
  margin-top: 3px;
  text-transform: uppercase;
}

.selesai {
  text-decoration: line-through;
  color: #95a5a6;
  opacity: 0.7;
}

input[type="checkbox"] {
  transform: scale(1.2);
  accent-color: #00b894;
  cursor: pointer;
}

.filter {
  margin: 1.25rem 0;
  text-align: center;
}

.filter label {
  font-weight: 500;
  color: #212529;
  text-transform: uppercase;
}

.pengingat-btn,
.hapus-btn {
  background: none;
  border: none;
  font-size: 1rem;
  cursor: pointer;
  transition: color 0.3s;
  text-transform: uppercase;
}

.pengingat-btn {
  color: #00b894;
}
.pengingat-btn:hover {
  color: #019270;
}

.hapus-btn {
  color: #e74c3c;
}
.hapus-btn:hover {
  color: #c0392b;
}
</style>
