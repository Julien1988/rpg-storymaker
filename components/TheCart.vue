<script lang="ts" setup>
const supabase = useSupabaseClient();
const props = defineProps<{
  type: object;
}>();

const src: any = ref("");
const getCarts = async (typeId: id) => {
  let { data } = await supabase
    .from("storymaker")
    .select(`id, image_url, carte_type_id, carte_type(name)`)
    .eq("carte_type_id", typeId);
  if (data?.length) {
    const randomNumber = Math.floor(Math.random() * data?.length);
    await downloadImage(data[randomNumber]);
  }
};

const downloadImage = async (cart) => {
  try {
    const { data, error } = await supabase.storage
      .from(cart.carte_type.name)
      .download(cart.image_url);
    if (error) throw error;
    src.value = URL.createObjectURL(data);
  } catch (error) {
    console.error("Error downloading image: ", error.message);
  }
};
await getCarts(props.type.id);
const reloadCart = () => {
  getCarts(props.type.id);
};
</script>

<template>
  <div>
    <img @click="reloadCart" alt="img" :src="src" class="lg:w-3/4" />
  </div>
</template>

<style scoped></style>
