<script setup lang="ts">
const supabase = useSupabaseClient();
const email = ref("");

const signInWithOtp = async () => {
  const { error } = await supabase.auth.signInWithOtp({
    email: email.value,
    options: {
      /*TODO JBR : modifier en prod */
      emailRedirectTo: "http://localhost:3000/confirm",
    },
  });
  if (error) console.log(error);
};
</script>
<template>
  <div>
    <div>
      <h2 class="text-4xl py-8">Uniquement sur invitation</h2>
    </div>
    <div>
      <button
        class="p-2 rounded bg-white text-blue-400 font-bold mr-2"
        @click="signInWithOtp"
      >
        Se connecter via E-Mail
      </button>
      <input v-model="email" type="email" class="p-2 text-blue-400" />
    </div>
  </div>
</template>
