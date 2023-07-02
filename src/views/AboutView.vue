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
      const matchingHour = this.weatherData[nowHourInt]
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
    axios.get('https://api.open-meteo.com/v1/forecast?latitude=35.69&longitude=139.69&hourly=weathercode&forecast_days=1&timezone=Asia%2FTokyo')
      .then(response => {
        console.log('response.data.hourly.weathercode', response.data.hourly.weathercode)
        this.weatherData = response.data.hourly.weathercode;
        this.nowHour = format(new Date(), "k時")
        this.collationHour()
        const collation = this.collationHour()
        const weather = this.nowWeatherCode
        this.weather = collation

        //todo  照合完了。ifのなかでmethodを呼び、処理を進める。
        if (weather === 0 || weather === 1) {
          console.log('晴れ');
        } else if (weather === 2 || weather === 3) {
          console.log('曇り');
        } else if (weather === 51 || weather === 53 || weather === 55 || weather === 56 || weather === 57) {
          console.log('霧雨');
        } else if (weather === 61 || weather === 63 || weather === 65 || weather === 66 || weather === 67) {
          console.log('雨');
        } else if (weather === 71 || weather === 73 || weather === 75 || weather === 77) {
          console.log('降雪');
        } else if (weather === 81 || weather === 82 || weather === 83) {
          console.log('にわか雨');
        } else if (weather === 85 || weather === 86) {
          console.log('にわか雪');
        } else if (weather === 95 || weather === 96 || weather === 99) {
          console.log('雷雨');
        } else {
          console.log('曇り');
        }

      })
      .catch(error => {
        console.error('Error:', error);
      });
    console.log("Created method finished");
  }

}
</script>

