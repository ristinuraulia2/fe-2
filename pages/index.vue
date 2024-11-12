<template>
  <div class="container-fluid d-flex justify-content-center align-items.center" style="background-color: aliceblue;">
    <div class="row">
      <div class="col-2 p-2 m-5">
        <div class="card shadow p-3 rounded-4">
          <h2 class="text-center text-white mb-4">Login</h2>
          <form @submit.prevent="login" id="loginForm">
            <div class="mb-3 ms-4">
              <!-- <label>email</label> -->
              <input v-model="email" type="email" id="email" name="email" class="rounded-3" placeholder="email">
            </div>
            <div class="mb-3 ms-4">
              <!-- <label>password</label> -->
              <input v-model="password" type="password" id="password" name="password" class="rounded-3"
                placeholder="password"><br>
            </div>
            <p>{{ message }}</p>
            <div class="vstack gap-2 col-md-5 mx-auto mt-5">

              <button type="submit" class="btn btn-outline-success">login</button>
            </div>
          </form>

        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useSupabaseClient, useRouter } from '#imports';

const supabase = useSupabaseClient()
const email = ref('');
const password = ref('');
const message = ref('');
const router = useRouter();

const login = async () => {
  try {
    const { data: user, error } = await supabase
      .from('login')
      .select('id, email, password, hakakses')
      .eq('email', email.value)
      .single();

    if (error || !user || user.password !== password.value) {
      message.value = 'Email atau password salah';
      return;
    }

    if (user.hakakses === 'walikelas') {
      router.push('/walikelas');

    } else if (user.hakakses === 'siswa') {
      router.push('/siswa');
    } else {
      message.value = 'Hak akses tidak dikenali';
    }

  } catch (err) {
    console.error('Error login:', err);
    message.value = 'Terjadi kesalahan';
  }
};
</script>


<style scoped>
.container-fluid {
  height: 100vh;
}

.card {
  height: 300px;
  width: 300px;
  background-color: #6EAF72;
}
</style>