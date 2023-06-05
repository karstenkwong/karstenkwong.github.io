<template>
  <section class="section">

    <b-carousel :indicator-inside="false" :interval="60000">
      <b-carousel-item>

        <img src="~assets/1.webp" style="{filter: blur(3px);
  -webkit-filter: blur(3px);}">

      </b-carousel-item>
      <b-carousel-item>

        <img src="~assets/2.webp" style="{filter: blur(3px);
-webkit-filter: blur(3px);}">

      </b-carousel-item>
      <b-carousel-item>

        <img src="~assets/3.webp" style="{filter: blur(3px);
-webkit-filter: blur(3px);}">

      </b-carousel-item>
      <b-carousel-item>

        <img src="~assets/4.webp" style="{filter: blur(3px);
-webkit-filter: blur(3px);}">
      </b-carousel-item>

      


    </b-carousel>
    <div class="box">
        <div id="clock">
          <p class="date" :key="updateKey + 1">{{ date }}</p>
          <p class="time" :key="updateKey">{{ time }}</p>
          <div class="columns" style="margin-top: -4em;">
            <div class="column">
              <p class="text" v-if="weather">{{ ` ${weather.temperature.data[19].place}:
                              ${weather.temperature.data[19].value}°C` }}</p>
            </div>
            <div class="column"><img v-if="weather" width="20%"
                :src="`https://www.hko.gov.hk/images/HKOWxIconOutline/pic${weather.icon[0]}.png`"></div>
          </div>


        </div>
      </div>
  </section>
</template>

<script>
import Card from '~/components/Card'
import CustomClock from '~/components/CustomClock'

export default {
  name: 'IndexPage',
  components: {
    Card,
    CustomClock
  },
  data() {
    return {
      time: '',
      date: '',
      week: ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT'],
      updateKey: Math.random(),
      weather: '',
    }
  },
  methods: {
    updateTime() {
      var cd = new Date();
      this.time = this.zeroPadding(cd.getHours(), 2) + ':' + this.zeroPadding(cd.getMinutes(), 2) + ':' + this.zeroPadding(cd.getSeconds(), 2);
      this.date = this.zeroPadding(cd.getFullYear(), 4) + '-' + this.zeroPadding(cd.getMonth() + 1, 2) + '-' + this.zeroPadding(cd.getDate(), 2) + ' ' + this.week[cd.getDay()];
      this.updateKey = Math.random()
    },
    zeroPadding(num, digit) {
      var zero = '';
      for (var i = 0; i < digit; i++) {
        zero += '0';
      }
      return (zero + num).slice(-digit);
    },
    getImgUrl(value) {
      return `~assets/${value}.webp`

    },
    async updateWeather() {
      await fetch('https://data.weather.gov.hk/weatherAPI/opendata/weather.php?dataType=rhrread&lang=tc')
        .then(async (response) => {
          this.weather = await response.json()
          console.log(this.weather);
        })
        .catch((error) => {
          console.log(`Error: ${error}`);
        })
    }
  },
  async mounted() {
    setInterval(() => {
      this.updateTime()

    }, 1000);

    setInterval(async () => {
      this.updateWeather()
    }, 1000 * 60);



    this.updateWeather()
    this.updateTime();
  }
}
</script>

<style lang="scss">
html,
body {
  height: 100%;
}

body {
  background: #0f3854;
  background: radial-gradient(ellipse at center, #0a2e38 0%, #000000 70%);
  background-size: 100%;
}

p {
  margin: 0;
  padding: 0;
}

#clock {
  font-family: 'Share Tech Mono', monospace;
  color: #ffffff;
  text-align: center;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  color: #ffffff;
  text-shadow: 0 0 20px rgb(0, 167, 223), 0 0 20px rgba(10, 175, 230, 0);

  .time {
    letter-spacing: 0.05em;
    font-size: 140px;
    padding: 5px 0;
    margin-top: -0.4em;
  }

  .date {
    letter-spacing: 0.1em;
    font-size: 40px;
    margin-top: 4em;
  }

  .text {
    margin-top: 0.3em;
    letter-spacing: 0.1em;
    font-size: 30px;
    padding: 2px 0 0;
  }
}
</style>
