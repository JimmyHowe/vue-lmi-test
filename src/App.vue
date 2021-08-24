<script>

import {reactive, onMounted} from "vue"
import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios'

const URL = "https://api.lmiforall.org.uk/api/v1/vacancies/search?limit=10&keywords=Developer";

export default {

components: {
  HelloWorld
},

setup()
{
  const data = reactive({
    results: [],
  });

  onMounted(async ()=>{

    try {
      
      let response = await axios.get(URL);

      data.results = response.data

    } catch (e) {
      
      console.log(e);
    }
  })

  function toHumanReadble(iso)
  {
    try {

      var time = new Date(iso).toLocaleDateString();

      console.log(time);

      return time;
    } catch (e)
    {
      console.log(e.message);
    }
  }

  return {
    data,
    toHumanReadble
  }
}

}
</script>

<template>
  
  <div class="container mx-auto p-4">
  
  <h1 class="text-4xl font-bold mt-8">
  Developer Jobs
  </h1>

  <div v-if="data.results.length === 0">
    LOADING...
  </div>

  <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4 mt-8">
    
    <a v-for="result in data.results" :key="result.id" :href="result.link"
      class="border rounded p-4"
     >
    
      <div class="font-semibold text-lg mb-4">
      {{ result.title }} - {{ result.company }}
      </div>

      <div>
      
        <div v-if="result.location.location">
          
          <strong>Location:</strong> {{ result.location.location }}

        </div>
      
      </div>

      <div>
      
        <div v-if="result.activedate.start">
          
          <strong>Start Date:</strong> {{ toHumanReadble(result.activedate.start) }}

        </div>
      
      </div>

    </a>

  </div>

  </div>

</template>