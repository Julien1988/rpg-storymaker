<script lang="ts" setup>
const supabase = useSupabaseClient();
const user = useSupabaseUser();
const logout = () => {
  supabase.auth.signOut();
};
</script>

<template>
  <header>
    <div v-if="user" class="flex flex-auto">
      <ul class="flex flex-auto">
        <li>
          <NuxtLink
            class="p-2 rounded bg-white text-blue-400 font-bold mr-2"
            to="/"
            >Acceuil</NuxtLink
          >
        </li>
        <li>
          <NuxtLink
            class="p-2 rounded bg-white text-blue-400 font-bold mr-2"
            to="/profile/account"
            >Profile</NuxtLink
          >
        </li>
      </ul>

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
