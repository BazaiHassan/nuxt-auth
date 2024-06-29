<template>
    <div>
        <NuxtLink to="/register">Register</NuxtLink>
        <br>
        <NuxtLink to="/login">Login</NuxtLink>
        <br>
        <NuxtLink v-if="user" to="/dashboard">Dashboard</NuxtLink>
        <br>
    </div>
</template>

<script setup lang="ts">
    const supabase = useSupabaseClient()
    const user = useSupabaseUser()

</script>