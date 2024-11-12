<template>
    <div class="container-fluid">
        <div class="row my-5">
            <div class="col-lg-6">
                <nuxt-link to="/piketwk/">
                    <div class="card bg-jadwalpiket rounded-5 text-black">
                        <div class="card-body">
                            <h2>Jadwal Piket Kelas</h2>
                        </div>
                    </div>
                </nuxt-link>
            </div>
            <div class="col-lg-6">
                <nuxt-link to="/Riwayatwk/">
                    <div class="card bg-riwayat rounded-5 text-black">
                        <div class="card-body">
                            <h2>Riwayat Piket</h2>
                        </div>
                    </div>
                </nuxt-link>
            </div>
        </div>
        <NuxtLink to="/">
            <button type="button" class="btn btn-success">Kembali</button>
        </NuxtLink>
    </div>
</template>
<style scoped>
.card {
    height: 170px;
    box-shadow: 1px 1px 10px #424242;
}

.card.bg-jadwalpiket {
    background-image: url("/assets/img/hijau.png");
    background-repeat: no-repeat;
    background-position: center center;
    background-size: cover;

}

.card.bg-riwayat {
    background: url("/assets/img/hijau.png") no-repeat center center;
    background-size: cover;
}
</style>