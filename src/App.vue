<script setup>
import { ref, onMounted } from 'vue'
import Navbar from './components/Navbar.vue'


</script>

<template>
  <main>
    <Navbar/>
    <section class="p-5 pt-24 lg:pt-[100px]">
      <div class="grid grid-cols-2 md:grid-cols-4 gap-2 ">
        <div class="h-15 aspect-[4/3] md:aspect-[3/4] xl:aspect-[4/3] rounded-lg md:col-start-4 md:row-start-2 md:flex md:self-center hidden">
          <span class="hidden lg:block">
            mluthfiamrullah
            Github
          </span>
        </div>
        <div class="h-15 aspect-[4/3] md:aspect-[3/4] xl:aspect-[4/3] rounded-lg md:col-start-2 md:row-start-2 md:flex md:self-end hidden">
          <span class="md:self-end">
            @luthfiamrullah
          </span>
        </div>
        <!-- <div class="h-15 bg-red-500 aspect-[4/3] md:aspect-[3/4] xl:aspect-[4/3] rounded-lg overflow-hidden group relative hover:scale-95 transition-all duration-500">
          <div class="w-full h-full bg-[url('/img/')] bg-cover bg-center absolute group-hover:scale-125 transition-all duration-500"></div>
        </div> -->
        <div class="h-15 bg-blue-500 aspect-[4/3] md:aspect-[3/4] xl:aspect-[4/3] rounded-lg"></div>
        <div class="h-15 bg-blue-500 aspect-[4/3] md:aspect-[3/4] xl:aspect-[4/3] rounded-lg md:col-start-3"></div>
        <div class="h-15 bg-blue-500 aspect-[4/3] md:aspect-[3/4] xl:aspect-[4/3] rounded-lg"></div>
        <div class="h-15 bg-blue-500 aspect-[4/3] md:aspect-[3/4] xl:aspect-[4/3] rounded-lg"></div>
        <div class="h-15 bg-blue-500 aspect-[4/3] md:aspect-[3/4] xl:aspect-[4/3] rounded-lg"></div>
        <div class="h-15 bg-blue-500 aspect-[4/3] md:aspect-[3/4] xl:aspect-[4/3] rounded-lg"></div>
        <div class="h-15 bg-blue-500 aspect-[4/3] md:aspect-[3/4] xl:aspect-[4/3] rounded-lg"></div>
        <div class="h-15 bg-blue-500 aspect-[4/3] md:aspect-[3/4] xl:aspect-[4/3] rounded-lg"></div>
      </div>
    </section>
  </main>
</template>
