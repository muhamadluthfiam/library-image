<script setup>
import { ref, onMounted } from 'vue'
import Navbar from './components/Navbar.vue'

const userData = ref(null);

onMounted(() => {
  const storedData = localStorage.getItem('uploadedImages');

  userData.value = storedData ? JSON.parse(storedData) : null;
});

</script>

<template>
  <main>
    <Navbar/>
    <section class="p-5 pt-24 lg:pt-[100px]">
      <div class="grid grid-cols-2 md:grid-cols-4 gap-2 ">
        <div class="h-15 aspect-[4/3] md:aspect-[3/4] rounded-lg overflow-hidden group relative hover:scale-95 transition-all duration-500" v-for="(image, i) in userData" :key="i">
          <img :src="image" alt="" class="w-full h-full bg-cover bg-center absolute group-hover:scale-125 transition-all duration-500">
        </div>
      </div>
    </section>
  </main>
</template>
