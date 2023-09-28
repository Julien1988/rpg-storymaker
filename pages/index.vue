<script lang="ts" setup>
const supabase = useSupabaseClient();
const src = ref();

const cartType = ref("");
const dataDb = ref([]);

const carteTypes = await supabase.from("carte_type").select("id, name");
const storyCart = ref([]);
const imgUrl = ref("");

const images = ref([]);

const downloadImage = async () => {
  try {
    const { data, error } = await supabase.storage
      .from(cartType.value)
      .download(imgUrl.value);
    if (error) throw error;

    src.value = URL.createObjectURL(data);
  } catch (error) {
    console.error("Error downloading image: ", error.message);
  }
};
const getCarts = async (id) => {
  let { data } = await supabase
    .from("storymaker")
    .select(`id, image_url, carte_type_id, carte_type(name)`)
    .eq("carte_type_id", id);
  dataDb.value.push(data);
  const count = Object.keys(data).length;
  const randomNumber = Math.floor(Math.random() * count);
  const getCart = data[randomNumber];
  cartType.value = getCart.carte_type.name;
  imgUrl.value = getCart.image_url;
  storyCart.value.push(getCart);
  await downloadImage();
  images.value.push(src.value);
};

carteTypes.data?.forEach((e) => {
  getCarts(e.id);
});

const reload = () => {
  images.value = [];

  carteTypes.data?.forEach((e) => {
    getCarts(e.id);
  });

  const count = Object.keys(dataDb.value).length;
  const randomNumber = Math.floor(Math.random() * count);
  const getProxyCarts = new Proxy(...dataDb.value);
  const getCart = getProxyCarts[randomNumber];
  cartType.value = getCart.carte_type.name;
  imgUrl.value = getCart.image_url;
  storyCart.value.push(getCart);
  downloadImage();
  images.value.push(src.value);
};
</script>

<template>
  <h1 class="text-4xl py-6">Le destin qui vous attend :</h1>
  <ul class="flex flex-auto flex-col md:flex-row flex-wrap">
    <li v-for="image in images" class="flex justify-center py-2">
      <img alt="img" :src="image" class="lg:w-3/4" />
    </li>
  </ul>
  <button @click="reload">Reload</button>
</template>

<style scoped></style>
