<template>
     <div v-for="city in savedCities" :key="city.id">
     <CityCard :city="city" @click="goToCityView(city)"/>
     </div>
   
</template>
    
<script setup>
import { ref } from 'vue';
import axios from 'axios'
import CityCard from './cityCard.vue';
import { useRouter } from 'vue-router';

const savedCities = ref([])

const getCities = async() => {
     if(localStorage.getItem('savedCities')){
          savedCities.value = JSON.parse(localStorage.getItem('savedCities'))
     }

     const request = []
     savedCities.value.forEach((city) => {
          request.push(
               axios.get(
                    `https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=imperial`
               )
          )
     })
  
     const weatherDate = await Promise.all(request)
     // fikcer delay
     await new Promise((res) => setTimeout(res,1000))
     weatherDate.forEach((value,index)=> {
          savedCities.value[index].weather = value.data
     })
}
await getCities()
const goToCityView = (city) => {
     router.push(
     {
          name:"cityView",
          params:{state:city.state,city:city.city},
          query:{
               id:city.id,
               lat:city.coords.lat,
               lng:city.coords.lng
          }
     }
     )
}
const router = useRouter()

</script>
    
<style>
    
</style>