<script lang="ts" setup>
const supabase = useSupabaseClient();
let test = await supabase.storage.from("avatars").getPublicUrl();
let { data } = await supabase.from("profiles").select(`*`).single();
console.log(data);
</script>

<template>
  <div>Page: index</div>
  <img :src="data.avatar_url" />
</template>

<style scoped></style>
