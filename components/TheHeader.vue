<script lang="ts" setup>
const supabase = useSupabaseClient();
const user = useSupabaseUser();
const logout = () => {
  supabase.auth.signOut();
};
</script>

<template>
  <header>
    <div v-if="user">
      <button
        class="p-2 rounded bg-white text-blue-400 font-bold mr-2"
        @click="logout"
      >
        Logout
      </button>
    </div>
  </header>
</template>

<style scoped></style>
