<template>
  <div class="weather" :class="weatherClass">
    <div class="container">
      <div class="card weather-form">
        <input 
          type="text" 
          class="weather-form__input" 
          v-model="searchQuery" 
          placeholder="Enter city" 
          @keyup.enter="weahterSearch"
         />
        <button class="weather-form__btn" @click="weahterSearch">
          Search
        </button>
      </div>
      <div class="card weather-load" v-if="isLoading">
        Loading...
      </div>
      <div class="weather-info" v-show="!isError && location && temperature !== 0 && description">
        <div class="weather-info__text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }} ºC</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>
      <div class="weather-bg">
        <div>
          <img class="weather-bg__img bg" src="./assets/bg.webp" alt="">
          <img class="weather-bg__img overcast" src="./assets/bg-1.jpg" alt="">
          <img class="weather-bg__img partly-cloudy" src="./assets/bg-2.jpg" alt="">
          <img class="weather-bg__img sunny" src="./assets/bg-3.jpg" alt="">
        </div>
      </div>
    </div>
  </div>
</template>
<!-- old methods -->
<!-- <script>
  export default{
    data(){
      return{
        location: '',
        temperature: 0,
        description: '',
        isLoading: false,
        isError: true,
        searchQuery: '',
      }
    },
    computed: {
      weatherClass(){
        if(this.description.includes("Sunny")){
          return 'sunny';
        } else if(this.description.includes('Overcast')){
          return 'overcast';
        } else if(this.description.includes('Partly Cloudy') || this.description.includes('Partly cloudy')){
          return 'partly-cloudy';
        } else{
          return '';
        }
      }
    },
    methods: {
      weahterSearch(){
          this.isLoading = true;
          this.isError = false;
          fetch(`http://api.weatherapi.com/v1/current.json?key=63d404d7bd794d1e931191259240205&q=${this.searchQuery}`)
          .then(res => res.json())
          .then(data =>{
            this.isLoading = false;
            this.location = data.location.name;
            this.temperature = data.current.temp_c;
            this.description = data.current.condition.text;
            this.resetSearchQuery();
          })
          .catch(error =>{
            this.isLoading = false;
            this.isError = true;
            console.error(error);
          })
      },
      resetSearchQuery(){
        this.searchQuery = '';
      }
    }
  }
</script> -->

<script setup>
import { computed, defineModel, ref } from 'vue';

  let location = '';
  let temperature = '';
  let description = ref(''); 
  let isLoading = ref(false);
  let isError = ref(false);
  let searchQuery = ref('');

  const weatherClass = computed(() =>{
    if(description.value.includes("Sunny")){
        return 'sunny';
      } else if(description.value.includes('Overcast')){
        return 'overcast';
      } else if(description.value.includes('Partly Cloudy') || description.value.includes('Partly cloudy')){
        return 'partly-cloudy';
      } else{
        return '';
      }
  })

  function weahterSearch(){
    console.log('searchQuery.value', searchQuery.value);
    isLoading.value = true;
    isError.value = false;
      fetch(`http://api.weatherapi.com/v1/current.json?key=63d404d7bd794d1e931191259240205&q=${searchQuery.value}`)
      .then(res => res.json())
      .then(data =>{
        isLoading.value = false;
        location = data.location.name;
        temperature = data.current.temp_c;
        description.value = data.current.condition.text;
        resetSearchQuery();
      })
      .catch(error =>{
        isLoading.value = false;
        isError.value = true;
        console.error(error);
      })
  };

  function resetSearchQuery(){
    searchQuery.value = '';
  }

</script>