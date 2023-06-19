<script setup>
import { onMounted, reactive, ref } from 'vue'

let background = ref('hot')
let query = ref('London')
let weatherData = ref(null)
//http://api.weatherapi.com/v1/current.json?key=b3ecb471be1d47638db193536231706&q=london
onMounted(() => fetchWeather())
function getTemperatureStatus(temperature) {
  if (temperature > 30) {
    return 'hot'
  } else if (temperature < 15) {
    return 'cold'
  } else {
    return 'normal'
  }
}

function fetchWeather() {
  weatherData.value = null
  fetch(
    `http://api.weatherapi.com/v1/current.json?key=b3ecb471be1d47638db193536231706&q=${query.value}`
  )
    .then((res) => res.json())
    .then((data) => {
      weatherData.value = data
      background.value = getTemperatureStatus(data.current.temp_c)
    })
}
</script>

<template>
  <main :class="background">
    <div class="search-container">
      <input class="search transparent" v-model="query" placeholder="Enter City" />
      <button @click.prevent="fetchWeather" class="search-button transparent">&gt;</button>
    </div>
    <section id="temp-container" class="transparent" v-if="weatherData">
      <h1 id="temp-c">{{ weatherData.current.temp_c }} °c</h1>
      <h3 id="condition">{{ weatherData.current.condition.text }}</h3>
    </section>
    <section v-else>Loading...</section>
  </main>
</template>

<style>
body,
#app {
  margin: 0px;
  padding: 0px;
}

main {
  position: absolute;
  height: 100vh;
  width: 100vw;
  padding: 20px;
  margin: 0px;
  display: flex;
  justify-content: start;
  flex-direction: column;
  align-items: center;
  color: white;
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
  background-size: cover;
}

.search-container {
  display: block;
}

.search-button {
  height: 50px;
  aspect-ratio: 1/1;
  border-radius: 15px;
  font-size: 25px;
  margin-left: 10px;
  font-weight: bolder;
  color: white;
}

.search {
  border-radius: 25px 0 25px 0;
  height: 50px;
  width: 350px;
  font-size: 20px;
  padding-left: 10px;
}

.transparent {
  background-color: rgba(255, 255, 255, 0.5);
  border: white 2px solid;
  -webkit-backdrop-filter: blur(5px);
  backdrop-filter: blur(10px);
}

#temp-c {
  margin: 0px;
  font-size: 100px;
  color: white;
  font-weight: boldier;
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}

#temp-container {
  border-radius: 25px;
  margin-top: 25px;
  padding: 50px;
}

#condition {
  
}

.cold {
  background-image: url('@/assets/winter.jpg');
}

.mid-cold {
  background-image: url('@/assets/mid_cold.jpg');
}

.hot {
  background-image: url('@/assets/hot.jpg');
}

.mid-hot {
  background-image: url('@/assets/mid_hot.jpg');
}

.normal {
  background-image: url('@/assets/normal.jpg');
}
</style>
