<template>
  <div class="container">
    <div class="row">
      <div v-for="schedule in schedules" :key="schedule.id" class="col-md-4">
        <div class="card my-4">
          <div class="card-header bg-success text-white">
            <h3>{{ schedule.hari }}</h3>
          </div>
          <div class="card-body">
            <ol>
              <li v-for="student in schedule.Siswa">{{ student.nama }}</li>
            </ol>
          </div>
        </div>
      </div>
      <div>
        <NuxtLink to="/walikelas/">
          <button type="button" class="btn btn-success">Kembali</button>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const schedules = ref([]);
const students = ref([]);

async function getSchedules() {
  const { data, error } = await supabase.from("jadwal_piket").select(`*, Siswa(nama)`).order("id");
  if (error) throw error;
  if (data) schedules.value = data;
}

async function getSiswa() {
  const { data, error } = await supabase.from("Siswa").select(`*, jadwal_piket(*)`);
  if (error) throw error;
  if (data) students.value = data;
}

onMounted(() => {
  getSchedules();
});
</script>
