<template>
    <div>
        logout dari sistem. tunggu sebentar...
    </div>
</template>

<script setup>
const supabase = useSupabaseClient()

async function logout() {
    const { error } = await supabase.auth.signOut()

    if (!error) navigateTo('/')
}


onMounted(() => logout())
</script>