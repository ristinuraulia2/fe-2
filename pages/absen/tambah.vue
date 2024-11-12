<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-6 offset-md-3">
        <form @submit.prevent="kirimData">
          <div class="mb-3">
            <select v-model="schedule" class="form-control form-control-lg form-select rounded-3"
              @change="getStudentsBySchedule">
              <option disabled value="">Cari Hari</option>
              <option v-for="(schedule, i) in schedules" :key="i" :value="schedule.id">{{ schedule.hari }}</option>
            </select>
          </div>

          <div class="mb-3" v-if="students.length">
            <div class="row">
              <div class="col">Nama</div>
              <div class="col">Tugas</div>
              <div class="col">Melaksanakan</div>
            </div>

            <div v-for="(item, index) in form" :key="index" class="row">
              <div class="col-2">{{ getStudentName(item.siswa) }}</div>

              <div class="col-6">
                <span class="me-3">
                  <input type="radio" :id="'menyapu' + index" :value="true" v-model="item.tugas" />
                  <label class="ms-1" for="menyapu">menyapu</label>
                </span>
                <span class="me-3">
                  <input type="radio" :id="'mengepel' + index" :value="false" v-model="item.tugas" />
                  <label class="ms-1" for="mengepel">mengepel</label>
                </span>
              </div>

              <div class="col-4">
                <span class="me-3">
                  <input type="radio" :id="'ya' + index" :value="true" v-model="item.melaksanakan" />
                  <label class="ms-1" for="ya">Ya</label>
                </span>
                <span>
                  <input type="radio" :id="'tidak' + index" :value="false" v-model="item.melaksanakan" />
                  <label class="ms-1" for="tidak">Tidak</label>
                </span>
              </div>
            </div>
          </div>
          <button type="submit" class="btn btn-success btn-lg rounded-2 px-3">Kirim</button>
        </form>
        <NuxtLink to="/siswa">
          <div class="vstack gap-2 col-md-5 mx-auto">
            <button type="button" class="btn btn-outline-success">Kembali</button>
          </div>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const schedules = ref([])
const students = ref([])
const schedule = ref('')

// Initialize form as an array to hold multiple student entries
const form = ref([])

async function kirimData() {
  // Prepare data to insert
  const dataToInsert = form.value.map(item => ({
    siswa: item.siswa,
    melaksanakan: item.melaksanakan, // This should be a boolean
    tugas: item.tugas,               // This should be a boolean
  }));

  const { data, error } = await supabase
    .from('Piket')
    .insert(dataToInsert)

  if (!error) {
    navigateTo("/Riwayat")
  } else {
    console.error('Error inserting data:', error)
  }
}

const getStudentsBySchedule = async () => {
  const { data, error } = await supabase.from('Siswa').select('*, tugas_piket(nama)').eq('jadwal_piket', schedule.value)
  if (error) throw error
  if (data) {
    students.value = data
    // Create form entries for each student
    form.value = data.map(student => ({
      siswa: student.id,
      melaksanakan: null, // Initialize as null, will be filled with radio button
      tugas_piket: null,        // Initialize as null, will be filled with radio button
    }))
  }
}

const getSchedules = async () => {
  const { data, error } = await supabase.from('jadwal_piket').select().order('id')
  if (data) schedules.value = data
}

const getStudentName = (id) => {
  const student = students.value.find(s => s.id === id)
  return student ? student.nama : ''
}

onMounted(() => {
  getSchedules()
})
</script>