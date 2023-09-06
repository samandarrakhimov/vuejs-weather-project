<template>
   <header class="sticky top-0 shadow-lg bg-blue-800">
    <nav class="mx-6 w-100 flex  justify-between py-6 text-white">
        <RouterLink :to="{name:'home'}">
        <div class=" flex  items-center gap-3 flex-1">
            <i class="fa-solid fa-sun text-white text-2xl"></i>
            <p class=" text-2xl">The local Weather</p>
        </div>
    </RouterLink>
      <div class="flex gap-3">
         <i class="fa-solid fa-circle-info text-xl hover:text-teal-800 
         duration-150 cursor-pointer" @click="modalTogle"></i> 
         <i class="fa-solid fa-plus text-xl hover:text-teal-800 duration-150"
         @click="addCity"
         v-if="route.query.preview"></i>
      </div>
    </nav>
    <BaseModal :modalActive="modalActive" @close-modal="modalTogle">
    <div class=" text-black">
      <h1 class=" text-xl mb-1">Haqida</h1>
      <p class="mb-4">
        Mahalliy ob-havo sizga joriy va ob-havoni kuzatish imkonini beradi
            siz tanlagan shaharlarning kelajakdagi ob-havosi.
      </p>
      <h2 class=" text-2xl"> U qanday ishlaydi:</h2>
      <ol class=" list-decimal mb-4 list-inside">
        <li>
          Ismingizni kiritib, shahringizni qidiring
              qidiruv paneli.
        </li>
        <li>
          Natijalar ichida shaharni tanlang, bu talab qilinadi
           tanlovingiz uchun joriy ob-havoga.
        </li>
        <li>
          "+" belgisini bosish orqali shaharni kuzatib boring
              yuqori o'ng. Bu shaharni a da ko'rish uchun qutqaradi
              keyinroq bosh sahifada.
        </li>
      </ol>
      <h2 class=" text-2xl"> Shaharni olib tashlash</h2>
      <p>
        Agar siz endi shaharni kuzatishni xohlamasangiz, shunchaki tanlang
            bosh sahifadagi shahar. Pastki qismida
            sahifasida shaharni o'chirish imkoniyati bo'ladi.
      </p>
    </div>
    </BaseModal>
   </header>
</template>
<script setup>
import { RouterLink,useRoute, useRouter } from 'vue-router';
import BaseModal from './BaseModal.vue';
import { v4 as uuidv4 } from 'uuid';
import { ref } from 'vue';
const route = useRoute()
const router = useRoute()
const saveCity = ref([])

const addCity =()=>{
  if (localStorage.getItem('savedCities')) {
    saveCity.value = JSON.parse(localStorage.getItem('savedCities'))
  }

  const Obj = {
    id:uuidv4(),
    city:route.params.city,
    state:route.params.state,
    coords:{
      lat:route.query.lat,
      lng:route.query.lng
    }
  }
  saveCity.value.push(Obj)
  localStorage.setItem('savedCities',JSON.stringify(saveCity.value))

  let query = Object.assign({},route.query)
  query.id = Obj.id
  delete query.preview
   router.replace({query})
}
const  modalActive = ref(null)
const modalTogle = () => {
  modalActive.value = !modalActive.value
}
</script>
