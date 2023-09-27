<script lang="ts" setup>
const supabase = useSupabaseClient();
const user = useSupabaseUser();
const logout = () => {
  supabase.auth.signOut();
};
</script>

<template>
  <header>
    <div v-if="user"><button @click="logout">Logout</button></div>
  </header>
</template>

<style scoped></style>
