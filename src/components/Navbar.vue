<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'
const isOpen = ref(false)

const file = ref(null)
const allowedTypeFiles = ['image/jpeg', 'image/jpg', 'image/bmp', 'image/png', 'image/gif']
const maxFileSizeInBytes = 2 * 1024 * 1024
const handleFileUpload = (e) => {
  const files = e.target.files
  for (let i = 0; i < files.length; i++) {
    const fileType = files[i].type;
    const fileSize = files[i].size;

    if (allowedTypeFiles.includes(fileType) && fileSize <= maxFileSizeInBytes) {
      console.log(`File ${files[i].name} is valid.`);
    } else {
      console.log(`File ${files[i].name} is not valid. Check file type and size.`);
    }
  }
}


const onSubmit = () => {
  console.log('hello')
  axios.post("https://api.imgbb.com/1/upload?key=6bf50d5cb3ec3a5ec733f9be8f5c849f")
}
</script>


<template>
  <section class="relative">
    <div class="flex justify-between border-b-2 py-5 px-5 bg-white absolute w-full">
      <h3 class="text-2xl font-normal">
        Library <span class="font-extralight italic text-slate-300">Image</span>
      </h3>
      <div class="flex gap-2">
        <button type="button" class="border px-3 py-1 rounded-lg" @click="isOpen = !isOpen">Add</button>
        <button class="border px-3 py-1 rounded-lg">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
            <path stroke-linecap="round" stroke-linejoin="round" d="M12 3v2.25m6.364.386l-1.591 1.591M21 12h-2.25m-.386 6.364l-1.591-1.591M12 18.75V21m-4.773-4.227l-1.591 1.591M5.25 12H3m4.227-4.773L5.636 5.636M15.75 12a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0z" />
          </svg>
        </button>
      </div>
    </div>

    <div class="bg-slate-700 opacity-50 w-full h-full fixed z-10" v-if="isOpen" @click="isOpen = !isOpen"></div>
    <!-- Modal -->
    <div v-if="isOpen" class="z-30 flex items-center justify-center h-full fixed top-0 left-0 right-0 bottom-0">
      <div class="border border-black max-w-2xl bg-white p-8 rounded-xl relative">
        <p>
          Upload image 
        </p>
        <input type="file" multiple="multiple" @change="handleFileUpload" capture>
        <div class="flex justify-center">
          <button @click="isOpen = !isOpen" type="button" class="border border-black hover:bg-red-400 hover:text-white focus:ring-1 focus:ring-red-200 absolute -top-4 right-0 z-30 bg-white w-8 h-8 flex items-center justify-center rounded-full">
            <span class="mb-0.5">X</span>
          </button>
          <button type="button" @click="onSubmit" class="px-4 py-1 border rounded-lg text-white bg-green-500 mt-3 hover:shadow-md hover:bg-green-400">Add</button>
        </div>
        <!-- <div class="mt-5 overflow-x-scroll">
          <div class="flex space-x-4 p-4">
            <div class="h-52 w-4/12 px-2 flex-shrink-0 relative" >
              <button @click="isOpen = !isOpen" type="button" class="border border-black hover:bg-red-400 hover:text-white focus:ring-1 focus:ring-red-200 absolute -top-3 right-0 z-30 bg-white w-8 h-8 flex items-center justify-center rounded-full">
                <span class="mb-0.5">X</span>
              </button>
              <img src="../assets/img/1.jpeg" class="w-full h-full object-cover object-center" alt="">
            </div>
            <div class="h-52 w-4/12 px-2 flex-shrink-0 relative" >
              <button @click="isOpen = !isOpen" type="button" class="border border-black hover:bg-red-400 hover:text-white focus:ring-1 focus:ring-red-200 absolute -top-3 right-0 z-30 bg-white w-8 h-8 flex items-center justify-center rounded-full">
                <span class="mb-0.5">X</span>
              </button>
              <img src="../assets/img/2.jpeg" class="w-full h-full object-cover object-center" alt="">
            </div>
            <div class="h-52 w-4/12 px-2 flex-shrink-0 relative" >
              <button @click="isOpen = !isOpen" type="button" class="border border-black hover:bg-red-400 hover:text-white focus:ring-1 focus:ring-red-200 absolute -top-3 right-0 z-30 bg-white w-8 h-8 flex items-center justify-center rounded-full">
                <span class="mb-0.5">X</span>
              </button>
              <img src="../assets/img/3.jpeg" class="w-full h-full object-cover object-center" alt="">
            </div>
            <div class="h-52 w-4/12 px-2 flex-shrink-0 relative" >
              <button @click="isOpen = !isOpen" type="button" class="border border-black hover:bg-red-400 hover:text-white focus:ring-1 focus:ring-red-200 absolute -top-3 right-0 z-30 bg-white w-8 h-8 flex items-center justify-center rounded-full">
                <span class="mb-0.5">X</span>
              </button>
              <img src="../assets/img/4.jpeg" class="w-full h-full object-cover object-center" alt="">
            </div>
            <div class="h-52 w-4/12 px-2 flex-shrink-0 relative" >
              <button @click="isOpen = !isOpen" type="button" class="border border-black hover:bg-red-400 hover:text-white focus:ring-1 focus:ring-red-200 absolute -top-3 right-0 z-30 bg-white w-8 h-8 flex items-center justify-center rounded-full">
                <span class="mb-0.5">X</span>
              </button>
              <img src="../assets/img/4.jpeg" class="w-full h-full object-cover object-center" alt="">
            </div>
          </div>
        </div> -->
      </div>
    </div>
    <!-- End Of Modal -->
  </section>
</template>
