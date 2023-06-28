<template>
  <div>
    <h1>東京の今日の天気</h1>
    <div v-if="nowWeatherCode">
      <p>24weatherData: now code is 『{{ nowWeatherCode }}』</p>
    </div>
    <div v-else>
      <p>天気情報を取得中...</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { format } from 'date-fns'

export default {
  data() {
    return {
      weatherData: [],
      nowHour: null,
      nowWeatherCode: null,
    }
  },
  methods: {
    collationHour() {
      const nowHourInt = parseInt(this.nowHour)
      const matchingHour = this.weatherData.find(hour => hour === nowHourInt)

      if (matchingHour !== undefined) {
        console.log('collationHour success', matchingHour)
        return matchingHour;
      } else {
        console.log('No matching hour found');
        return null;
      }
    },
  },
  async created() {
    console.log("Created method started")
    console.log(format(new Date(), 'k時'))
    // => 2022年6月3日
    axios.get('https://api.open-meteo.com/v1/forecast?latitude=35.69&longitude=139.69&hourly=weathercode&forecast_days=1&timezone=Asia%2FTokyo')
      .then(response => {
        console.log('response.data.hourly.weathercode', response.data.hourly.weathercode)
        this.weatherData = response.data.hourly.weathercode;
        this.nowHour = format(new Date(), "k")
        this.collationHour()
        const collation = this.collationHour()
        this.nowWeatherCode = collation

        //todo  できてるやん！！ log部分をthis.methodにして、それぞれのmethodでアニメーションを実装すれば良い
        if (this.nowWeatherCode === 1) {
          console.log('Weather code is 1');
        } else if (this.nowWeatherCode === 2) {
          console.log('Weather code is 2');
        } else if (this.nowWeatherCode === 3) {
          console.log('Weather code is 3');
        } else {
          console.log('Weather code is not 1, 2, or 3');
        }

      })
      .catch(error => {
        console.error('Error:', error);
      });
    console.log("Created method finished");
  }

}
</script>

