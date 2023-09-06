<template>
    <main class="mx-6 text-white">
        <div class="mb-8 pt-4 relative">
     <input type="text" placeholder="Shahar yoki shtatni qidiring"
     class="py-2 px-1 w-full text-white bg-transparent border-b focus: bg-teal-800 focus:
      outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      v-model="searchQuery"
      @input="querySearchResults">
      <p class="py-2" v-if="searchErorr">
        Kechirasiz, nimadir noto‘g‘ri ketdi, qayta urinib ko‘ring.
    </p>
    <p class="py-2" v-if="!searchErorr && mapboxSearchResults?.length === 0">
        Soʻrovingizga mos natija topilmadi, boshqa shartni kiriting.
    </p>
   <template v-else>
    <ul v-if="mapboxSearchResults" class=" absolute bg-teal-900 text-white 
    w-full shadow-md top-[66px] py-2 px-1">
    <li v-for="searchResult in mapboxSearchResults" :key="searchResult.id"
    class=" py-2 cursor-pointer" @click="priewCity(searchResult)">
    {{ searchResult.place_name }}
    </li>
   </ul>
   </template>
    </div>
    <div class="flex flex-1 gap-4 flex-col">
        <Suspense>
            <CityList/>
            <template #fallback>
                <CityCardSkelteon/>
            </template>
        </Suspense>
    </div>
   
    </main>
</template>
<script setup>
import { ref } from 'vue';
import axios from 'axios'
import { useRouter } from 'vue-router';
import CityList from '../components/cityList.vue';
import CityCardSkelteon from '../components/cityCardSkelteon.vue';
const router = useRouter()

const priewCity = (searchResult) => {
console.log(searchResult);
const [city,state] = searchResult.place_name.split(",")
console.log(city,state);
router.push({
    name:'cityView',
    params:{state:state,city:city},
    query:{
        lat:searchResult.geometry.coordinates[1],
        lng:searchResult.geometry.coordinates[0],
        preview:true
    }
})
}

const mapboxAPIKey =  
"pk.eyJ1Ijoiam9obmtvbWFybmlja2kiLCJhIjoiY2t5NjFzODZvMHJkaDJ1bWx6OGVieGxreSJ9.IpojdT3U3NENknF6_WhR2Q";

const searchQuery = ref("")
const queryTimeout = ref(null)
const mapboxSearchResults = ref(null)
const searchErorr = ref(null)

const querySearchResults = () => {
    clearTimeout(queryTimeout.value)
 queryTimeout.value = setTimeout(async() => {
    if (searchQuery.value !== "") {
       try{
        const results = await axios.get(
        `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`)
        mapboxSearchResults.value = results.data.features
       } catch{
        searchErorr.value = true
       }
        return
    }
    mapboxSearchResults.value = null

 },300)
}
</script>
