<template>
  <div class="container">
    <h1>Daftar Kegiatan</h1>

    <form @submit.prevent="tambahKegiatan">
      <input v-model="kegiatanBaru" placeholder="Nama kegiatan" required />
      <input v-model="waktuBaru" type="time" required />
      <button type="submit">Tambah</button>
    </form>

    <div class="filter">
      <label>
        <input type="checkbox" v-model="hanyaBelumSelesai" />
        Tampilkan hanya yang belum selesai
      </label>
    </div>

    <ul>
      <li v-for="(kegiatan, index) in kegiatanTersaring" :key="index">
        <input type="checkbox" v-model="kegiatan.selesai" />
        <div class="info">
          <span>{{ kegiatan.nama }}</span>
          <small>{{ kegiatan.waktu }}</small>
        </div>
        <button class="pengingat-btn" @click="togglePengingat(kegiatan)">
          <span v-if="kegiatan.pengingat">🔔</span>
          <span v-else>🔕</span>
        </button>
        <button class="edit-btn" @click="editKegiatan(index)">
          Edit
        </button>
        <button class="hapus-btn" @click="hapusKegiatan(index)">
          Hapus
        </button>
      </li>
    </ul>

    <div v-if="isEditing" class="edit-form">
      <h2>Edit Kegiatan</h2>
      <form @submit.prevent="updateKegiatan">
        <input v-model="editKegiatanBaru" placeholder="Nama kegiatan" required />
        <input v-model="editWaktuBaru" type="time" required />
        <button type="submit">Update</button>
        <button @click="cancelEdit">Cancel</button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const kegiatanBaru = ref('')
const waktuBaru = ref('')
const daftarKegiatan = ref([])
const hanyaBelumSelesai = ref(false)

const isEditing = ref(false)
const editIndex = ref(null)
const editKegiatanBaru = ref('')
const editWaktuBaru = ref('')

const tambahKegiatan = () => {
  if (kegiatanBaru.value.trim() && waktuBaru.value) {
    daftarKegiatan.value.push({
      nama: kegiatanBaru.value,
      waktu: waktuBaru.value,
      selesai: false,
      pengingat: false,
    })
    kegiatanBaru.value = ''
    waktuBaru.value = ''
  }
}

const togglePengingat = (kegiatan) => {
  kegiatan.pengingat = !kegiatan.pengingat
}

const hapusKegiatan = (index) => {
  daftarKegiatan.value.splice(index, 1)
}

const editKegiatan = (index) => {
  const kegiatan = daftarKegiatan.value[index]
  editIndex.value = index
  editKegiatanBaru.value = kegiatan.nama
  editWaktuBaru.value = kegiatan.waktu
  isEditing.value = true
}

const updateKegiatan = () => {
  if (editKegiatanBaru.value.trim() && editWaktuBaru.value) {
    daftarKegiatan.value[editIndex.value].nama = editKegiatanBaru.value
    daftarKegiatan.value[editIndex.value].waktu = editWaktuBaru.value
    cancelEdit()
  }
}

const cancelEdit = () => {
  isEditing.value = false
  editKegiatanBaru.value = ''
  editWaktuBaru.value = ''
}

const kegiatanTersaring = computed(() => {
  return hanyaBelumSelesai.value
    ? daftarKegiatan.value.filter(k => !k.selesai)
    : daftarKegiatan.value
})
</script>

<style scoped>
.container {
  max-width: 500px;
  margin: 2rem auto;
  padding: 1.5rem;
  background-color: #fff;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  color: #000;
}

h1 {
  text-align: center;
  margin-bottom: 1.25rem;
}

form {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  margin-bottom: 1rem;
}

input[type="text"],
input[type="time"] {
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

.filter {
  margin: 1rem 0;
  text-align: center;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 0.75rem;
  margin-bottom: 0.75rem;
  background-color: #f1f1f1;
  padding: 0.75rem;
  border-radius: 8px;
}

.info {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
}

.info small {
  font-size: 0.8rem;
  color: #333;
}

.pengingat-btn {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1.3rem;
  color: #007bff;
}

.pengingat-btn:hover {
  color: #0056b3;
}

.hapus-btn {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  color: #dc3545;
}

.hapus-btn:hover {
  color: #c82333;
}

.edit-btn {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  color: #28a745;
}

.edit-btn:hover {
  color: #218838;
}

.edit-form {
  margin-top: 1.5rem;
  padding: 1.5rem;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.edit-form input {
  margin-bottom: 1rem;
}

.edit-form button {
  margin-right: 0.5rem;
}
</style>
