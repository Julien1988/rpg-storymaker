<script setup>
const supabase = useSupabaseClient();

const loading = ref(true);
const username = ref("");
const website = ref("");
const avatar_path = ref("");

loading.value = true;
const user = useSupabaseUser();

let { data } = await supabase
  .from("profiles")
  .select(`username, website, avatar_url`)
  .eq("id", user.value.id)
  .single();

if (data) {
  username.value = data.username;
  website.value = data.website;
  avatar_path.value = data.avatar_url;
}

loading.value = false;

async function updateProfile() {
  try {
    loading.value = true;
    const user = useSupabaseUser();

    const updates = {
      id: user.value.id,
      username: username.value,
      website: website.value,
      avatar_url: avatar_path.value,
      updated_at: new Date(),
    };

    let { error } = await supabase.from("profiles").upsert(updates, {
      returning: "minimal", // Don't return the value after inserting
    });

    if (error) throw error;
  } catch (error) {
    alert(error.message);
  } finally {
    loading.value = false;
  }
}

async function signOut() {
  try {
    loading.value = true;
    let { error } = await supabase.auth.signOut();
    if (error) throw error;
  } catch (error) {
    alert(error.message);
  } finally {
    loading.value = false;
  }
}
</script>

<template>
  <form class="form-widget" @submit.prevent="updateProfile">
    <Avatar v-model:path="avatar_path" @upload="updateProfile" />
    <div class="py-2">
      <label for="email" class="py-2 pr-2">Email</label>
      <input
        id="email"
        class="py-2 pl-2"
        type="text"
        :value="user.email"
        disabled
      />
    </div>
    <div class="py-2">
      <label for="username" class="py-2 pr-2">Name</label>
      <input
        id="username"
        class="py-2 text-blue-400"
        type="text"
        v-model="username"
      />
    </div>
    <div class="py-2">
      <label for="website" class="py-2 pr-2">Website</label>
      <input
        id="website"
        class="py-2 text-blue-400"
        type="url"
        v-model="website"
      />
    </div>

    <div class="py-2">
      <input
        type="submit"
        class="button primary block py-2 pr-2 text-blue-400 bg-white rounded p-2"
        :value="loading ? 'Loading ...' : 'Update'"
        :disabled="loading"
      />
    </div>

    <div>
      <button
        class="button primary block py-2 pr-2 text-blue-400 bg-white rounded p-2"
        @click="signOut"
        :disabled="loading"
      >
        Sign Out
      </button>
    </div>
  </form>
</template>
