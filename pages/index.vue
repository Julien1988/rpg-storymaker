<script lang="ts" setup>
const supabase = useSupabaseClient();

const carteTypes = await supabase.from("carte_type").select("id, name");
</script>

<template>
  <h1 class="text-4xl py-6">Le destin qui vous attend :</h1>
  <p class="text-xs pb-2">Cliquer sur les cartes pour relancer celle-ci.</p>
  <ul class="flex flex-auto flex-col md:flex-row flex-wrap justify-center">
    <li v-for="type in carteTypes.data" class="flex justify-center py-2">
      <TheCart :type="type" />
    </li>
  </ul>
</template>

<style scoped></style>
