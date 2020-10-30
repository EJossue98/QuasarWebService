<template>
  <q-page class="flex column">
    
    <div class="col q-pt-lg q-px-sm">
      

      <img src="../statics/weather-map.gif" class="center" alt="Bridge" width="395" height="300" >

            <q-input v-model="search" @keyup.enter="getWeatherBySearch" placeholder="Buscar" dark borderless >
      
        <template v-slot:before>
          <q-icon @click="getLocation" name="my_location"/>
        </template>

        <template v-slot:append>
          <q-btn @click="getWeatherBySearch" round dense flat icon="search" />
        </template>
      </q-input>

    </div>

    <template v-if="weatherData"> 
    <div class="col text-white text-center">
      
      <div class="text-h4 text-weight-light">
       {{weatherData.name}}
      </div>

      <div class="text-h1 text-weight-thin q-my-lg relative-position">
        <span>
          {{Math.round(weatherData.main.temp)}}
        </span>
        <span class="text-h3 relative-position degree">
         &deg;
        </span>
        
      </div>
      
      <div class="text-h6 text-weight-light">
        {{weatherData.weather[0].main}}
      </div>
    </div>
    
    </template>
    <template v-else>
      <div class="col column text-center text-grey">
        
        <div class="col text-h1 ">
          Quasar 
        </div>
        <div class="col text-h2 ">
          WebService <br> Clima

        </div>
        <q-btn @click="getLocation" class="col" flat >
          <q-icon left size="3em" name="my_location" />
          <div>¿Cuál es el clima donde me encuentro?</div>
        </q-btn>
      </div>
    </template>

  </q-page>
</template>


<script>
export default {
  name: "PageIndex",
  data(){
    return{
      search:'',
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: 'https://api.openweathermap.org/data/2.5/weather',
      apiKey: 'e77f6757387b65be9143c80d943daab7'
    }
  },
  methods:{
    getLocation(){
      navigator.geolocation.getCurrentPosition(
        position => {
          console.log('position:', position)
          this.lat= position.coords.latitude
          this.lon= position.coords.longitude
          this.getWeatherByCoords()
        })
    },
    getWeatherByCoords(){
      this.$axios(`${ this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`).then(response=> {
        this.weatherData = response.data
      })
      
    },
    getWeatherBySearch(){
      console.log('getWeatherBySearch')
      this.$axios(`${ this.apiUrl}?q=${this.search}&appid=${this.apiKey}&units=metric`).then(response=> {
      this.weatherData = response.data
      })
      
       
    }
  }
};
</script>

<style lang="sass">
.q-page
  background: linear-gradient(to top, #000428, #004e92)
.degree
  top: -44px
</style>