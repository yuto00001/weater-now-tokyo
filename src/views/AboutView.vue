<template>
  <div>
    <h1>東京の今日の天気</h1>
    <div v-if="weatherData">
      <p>24weatherData: {{ weatherData }}</p>
    </div>
    <div v-else>
      <p>天気情報を取得中...</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      weatherData: []
    }
  },
  async created() {
    console.log("Created method started");
    axios.get('https://api.open-meteo.com/v1/forecast?latitude=35.69&longitude=139.69&hourly=weathercode&forecast_days=1&timezone=Asia%2FTokyo')
      .then(response => {
        console.log('Response:', response);
        console.log('response.data.', response.data)
        console.log('response.data.hourly.weathercode', response.data.hourly.weathercode)
        this.weatherData = response.data.hourly.weathercode;
      })
      .catch(error => {
        console.error('Error:', error);
      });
    console.log("Created method finished");
  }



}
</script>

