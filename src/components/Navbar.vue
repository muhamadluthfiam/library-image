<script setup>
import Swal from 'sweetalert2'
import axios from 'axios'
import { computed, onMounted, ref } from 'vue'
const isOpen = ref(false)

const file = ref(null)
const attachment = ref([])
const isLoading = ref(false)

const invalid = ref()

const allowedTypeFiles = ['image/jpeg', 'image/jpg', 'image/bmp', 'image/png', 'image/gif']
const maxFileSizeInBytes = 2 * 1024 * 1024


const showAlert = () => {
  Swal.fire({
    title: 'Success!',
    text: 'image successfully uploaded',
    icon: 'success',
    confirmButtonText: 'OK'
  })
}

const handleFileUpload = (e) => {
  const files = e.target.files
  attachment.value.push(...files)
  for (let i = 0; i < files.length; i++) {
    const fileType = files[i].type;
    const fileSize = files[i].size;

    if (allowedTypeFiles.includes(fileType) && fileSize <= maxFileSizeInBytes) {
      const reader = new FileReader()
      reader.onload = async (event) => {
        attachment.value[i].preview = event.target.result;
      };
      isLoading.value = true     
      setTimeout(() => {
        isLoading.value = false     
      }, 3000)
      reader.readAsDataURL(files[i]);
    } else {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: `File ${files[i].name} is not valid. Check file type and size.`,
        confirmButtonText: 'OK'
      });
      isOpen.value = false
    }
  }
}


const removeAttachment = (e) => {
  console.log(attachment.value)
  attachment.value.splice(e, 1)
}

const onSubmit = async () => {
  try {
    isLoading.value = true;

    const base64Images = attachment.value.map((file) => {
      return new Promise((resolve) => {
        const reader = new FileReader();
        reader.onload = () => resolve(reader.result);
        reader.readAsDataURL(file);
      });
    });

    const imageDataArray = await Promise.all(base64Images);

    const imgbbApiKey = 'd7e47b86f195b0aa31933f2cabce40ca';
    const imgbbApiUrl = `https://api.imgbb.com/1/upload?key=${imgbbApiKey}`;
    
    const imgUploadPromises = imageDataArray.map((base64Data) => {
      const formData = new FormData();
      formData.append('image', base64Data.split(',')[1]);

      return axios.post(imgbbApiUrl, formData, {
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded',
        },
      });
    });

    const imgbbResponses = await Promise.all(imgUploadPromises)

    const existingImages = JSON.parse(localStorage.getItem('uploadedImages')) || [];
    const newImgUrls = imgbbResponses.map((response) => response.data.data.url);
    const allImgUrls = [...existingImages, ...newImgUrls];
    localStorage.setItem('uploadedImages', JSON.stringify(allImgUrls))
    showAlert();
    attachment.value = [];
  } catch (error) {
    Swal.fire({
      icon: 'error',
      title: 'Oops...',
      text: 'Error uploading images. Please try again.',
      confirmButtonText: 'OK',
    });
  } finally {
    isLoading.value = false;
    isOpen.value = false;
  }
}
</script>


<template>
  <section class="relative">
    <div class="flex justify-between border-b-2 py-5 px-5 bg-white absolute w-full">
      <h3 class="text-2xl font-normal">
        Library <span class="font-extralight italic text-slate-300">Image</span>
      </h3>
      <div class="flex gap-2">
        <button type="button" class="border px-3 py-1 rounded-lg hover:bg-zinc-950 hover:text-white ring-1 ring-zinc-700 shadow-md" @click="isOpen = !isOpen">Upload</button>
      </div>
    </div>

    <div class="bg-slate-700 opacity-50 w-full h-full fixed z-10" v-if="isOpen" @click="isOpen = !isOpen"></div>
    <!-- Modal -->
    <div v-if="isOpen" class="z-30 flex items-center justify-center h-full fixed top-0 left-0 right-0 bottom-0">
      <div class="border border-black w-full lg:max-w-2xl bg-white p-8 rounded-xl relative">
        <div class="flex justify-center text-2xl">
          <p>
            Multiple Images
          </p>
        </div>
        <input type="file" multiple="multiple" @change="handleFileUpload" class="mt-5">
        <div class="flex justify-center">
          <button @click="isOpen = !isOpen" type="button" class="border border-black hover:bg-red-400 hover:text-white focus:ring-1 focus:ring-red-200 absolute -top-4 right-0 z-30 bg-white w-8 h-8 flex items-center justify-center rounded-full">
            <span class="mb-0.5">X</span>
          </button>
        </div>
        <div class="mt-5 overflow-x-scroll">
          <div v-if="isLoading" class="flex justify-center items-center">
            <svg class="animate-spin h-10 w-10" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" d="M16.023 9.348h4.992v-.001M2.985 19.644v-4.992m0 0h4.992m-4.993 0l3.181 3.183a8.25 8.25 0 0013.803-3.7M4.031 9.865a8.25 8.25 0 0113.803-3.7l3.181 3.182m0-4.991v4.99" />
            </svg>
          </div>
          <div class="flex space-x-4 py-10">
            <div class="h-52 w-full lg:w-4/12 px-2 flex-shrink-0 relative" v-show="!isLoading" v-for="(image, i) in attachment" :key="i">
              <button @click="removeAttachment(i)" type="button" class="border border-black hover:bg-red-400 hover:text-white focus:ring-1 focus:ring-red-200 absolute -top-3 right-0 z-30 bg-white w-8 h-8 flex items-center justify-center rounded-full">
                <span class="mb-0.5">X</span>
              </button>
              <img :src="image.preview" class="w-full h-full object-cover object-center" alt="">
              <p v-if="image.size > maxFileSizeInBytes" class="text-sm italic font-extralight text-red-400 text-center">
                image must be less than 2MB
              </p>
            </div>
          </div>
        </div>
        <div class="flex justify-center">
          <button type="button" @click="onSubmit" class="px-4 py-1 border rounded-lg text-white bg-green-500 mt-3 hover:shadow-md hover:bg-green-400">Submit</button>
        </div>
      </div>
    </div>
    <!-- End Of Modal -->
  </section>
</template>
