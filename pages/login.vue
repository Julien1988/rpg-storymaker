<script setup lang="ts">
const supabase = useSupabaseClient();
const email = ref("");
const isSend = ref(false);
const isError = ref(false);
const signInWithOtp = async () => {
  isSend.value = true;
  isError.value = false;
  const { error } = await supabase.auth.signInWithOtp({
    email: email.value,
    options: {
      /*TODO JBR : modifier en prod  http://localhost:3000/confirm */
      /* PROD : https://rpg-storycart.netlify.app/ */
      emailRedirectTo: "https://rpg-storycart.netlify.app/",
    },
  });
  if (error) {
    isSend.value = false;
    isError.value = true;
    console.log(error);
  }
};
</script>
<template>
  <div>
    <div>
      <h2 class="text-2xl py-8">Uniquement sur invitation</h2>
    </div>
    <div>
      <button
        class="p-2 rounded bg-white text-blue-400 font-bold mr-2"
        @click="signInWithOtp"
        v-if="!isSend"
      >
        Se connecter via E-Mail
      </button>
      <p v-else class="pt-4 animate-pulse">Un instant ...</p>
      <input v-model="email" type="email" class="p-2 text-blue-400 my-4" />
      <p v-if="isError" class="text-red-400 animate-pulse">
        Il semblerait qu'il y ait un problème.
      </p>
    </div>
  </div>
</template>
