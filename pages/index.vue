<script lang="ts" setup>
const supabase = useSupabaseClient();
const src = ref("");

const carteTypes = await supabase.from("carte_type").select("id, name");
const storyCart = ref([]);

const getCarts = async (id) => {
  let { data } = await supabase
    .from("storymaker")
    .select(`id, image_url, carte_type_id, carte_type(name)`)
    .eq("carte_type_id", id);

  //console.log(data[0].carte_type.name);
  storyCart.value[data[0].carte_type.name] = data;
};

carteTypes.data?.forEach((e) => {
  getCarts(e.id);
});

/*let imgUrl = data.image_url;*/
const downloadImage = async () => {
  try {
    const { data, error } = await supabase.storage
      .from("action")
      .download(imgUrl);
    if (error) throw error;
    src.value = URL.createObjectURL(data);
  } catch (error) {
    console.error("Error downloading image: ", error.message);
  }
};
//downloadImage();
</script>

<template>
  <div>Page: index</div>
  <img alt="img" :src="src" />
</template>

<style scoped></style>
