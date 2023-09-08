<template>

</template>
<script setup>
  import {useRoute} from "vue-router";
  import axios from "axios";

  const route = useRoute();
  const getWeatherData = async () =>{
    try {
      const weatherData = await axios.get(
          `https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=imperial`)
      console.log('weatherData 1', weatherData.data)

      // cal current date & time
      const localOffset = new Date().getTimezoneOffset() * 60000;
      const utc = weatherData.data.current.dt * 1000 + localOffset;
      weatherData.data.currentTime =
          utc + 1000 * weatherData.data.timezone_offset;

      // cal hourly weather offset
      weatherData.data.hourly.forEach((hour) => {
        const utc = hour.dt * 1000 + localOffset;
        hour.currentTime =
            utc + 1000 * weatherData.data.timezone_offset;
      });
      console.log('weatherData 2', weatherData.data)
      return weatherData.data
    }
    catch (err){
      console.log('Error Log',err)
    }
  }

  const weatherData = await getWeatherData()
</script>
