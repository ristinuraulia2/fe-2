<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">Riwayat Piket</h2>

        <!-- Form Pencarian -->
        <div class="my-3">
          <form @submit.prevent="getPiket">
            <!-- Input untuk mencari berdasarkan hari -->
            <div class="my-3">
              <input type="search" class="form-control form-control-lg rounded-5" placeholder="Cari hari (misal: Senin, Selasa, dll.)" v-model="keyword" />
            </div>

            <!-- Input untuk mencari berdasarkan tanggal -->
            <div class="my-3">
              <input type="date" class="form-control form-control-lg rounded-5" v-model="searchDate" placeholder="Pilih Tanggal" />
            </div>

            <div class="my-3 text-muted">Menampilkan {{ filteredVisitors.length }} dari {{ visitors.length }} data</div>
          </form>
        </div>

        <!-- Tabel untuk menampilkan riwayat piket -->
        <table class="table">
          <thead>
            <tr>
              <td>No</td>
              <td>Hari</td>
              <td>Tanggal</td>
              <td>Nama</td>
              <td>Tugas Piket</td>
              <td>Melaksanakan</td>
            </tr>
          </thead>
          <tbody>
            <!-- Menampilkan hanya data yang sesuai dengan filter -->
            <tr v-for="(visitor, i) in filteredVisitors" :key="i">
              <td>{{ i + 1 }}</td>
              <td>{{ visitor.Siswa?.jadwal_piket?.hari }}</td>
              <td>{{ visitor.tanggal }}</td>
              <td>{{ visitor.Siswa?.nama }}</td>
              <td>{{ visitor.Siswa?.tugas_piket?.nama || "Menyapu" }}</td>
              <td>
                {{ visitor.melaksanakan === null ? "" : visitor.melaksanakan ? "Ya" : "Tidak" }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <NuxtLink to="/siswa/">
        <button type="button" class="btn btn-success">Kembali</button>
      </NuxtLink>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
const supabase = useSupabaseClient();
const visitors = ref([]); // Menyimpan semua data
const keyword = ref(""); // Untuk mencari berdasarkan hari
const searchDate = ref(""); // Untuk mencari berdasarkan tanggal

// Fungsi untuk mendapatkan data piket
const getPiket = async () => {
  let query = supabase.from("Piket").select(
    `*,
      Siswa (
        nama,
        jadwal_piket( hari ), 
        tugas_piket( nama )
      )`,
    { count: "exact" }
  );

  // Menjalankan query untuk mengambil data
  const { data, error } = await query;

  if (error) {
    console.error("Error fetching data:", error);
    return;
  }

  if (data) {
    visitors.value = data;
  }
};

// Komputasi untuk memfilter data berdasarkan hari dan tanggal
const filteredVisitors = computed(() => {
  return visitors.value.filter((visitor) => {
    // Filter berdasarkan hari
    const isDayMatch = keyword.value ? visitor.Siswa?.jadwal_piket?.hari.toLowerCase().includes(keyword.value.toLowerCase()) : true;

    // Filter berdasarkan tanggal
    const isDateMatch = searchDate.value ? visitor.tanggal === searchDate.value : true;

    // Hanya tampilkan jika keduanya sesuai
    return isDayMatch && isDateMatch;
  });
});

// Memanggil fungsi getPiket saat komponen dimuat
onMounted(() => {
  getPiket();
});
</script>

<style scoped>
table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
}
</style>
