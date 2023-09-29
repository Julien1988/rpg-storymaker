<script lang="ts" setup>
const supabase = useSupabaseClient();
const user = useSupabaseUser();
const logout = () => {
  supabase.auth.signOut();
};
</script>

<template>
  <header class="py-6">
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Martian+Mono:wght@400;600&family=Permanent+Marker&display=swap"
      rel="stylesheet"
    />
    <NuxtLink to="/"
      ><h1 class="text-4xl text-blue-400 titleSite">
        RPG-STORYCART GENERATOR
      </h1></NuxtLink
    >
    <nav v-if="user" class="flex py-6">
      <ul class="flex flex-auto items-center">
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
    </nav>
  </header>
</template>

<style>
body {
  font-family: "Martian Mono", monospace;
}
.titleSite {
  font-family: "Permanent Marker", cursive;
}
</style>
