<script>
export default {
  components: {

  },
  data() {
    return {
      time: '',
      date: '',
      week: ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT'],
      updateKey: Math.random(),
      weather: undefined,
      klcWeather: undefined,
      fnd: undefined,
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

          for (let item of this.weather.temperature.data) {
            if (item.place == "九龍城" || item.place == "黃大仙") {
              this.klcWeather = item;
            }
          }
          //console.log(this.weather);
        })
        .catch((error) => {
          console.log(`Error: ${error}`);
        })

      await fetch('https://data.weather.gov.hk/weatherAPI/opendata/weather.php?dataType=fnd&lang=tc')
        .then(async (response) => {
          this.fnd = await response.json()
          //console.log(this.fnd);
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

<template>
  <video autoplay muted loop playsinline id="myVideo">
    <source src="../assets/wallpaper.mp4" type="video/mp4">
  </video>
  <section class="section">
    <div id="clock">
      <p class="date" :key="updateKey + 1">{{ date }}</p>
      <p class="time" :key="updateKey">{{ time }}</p>

      <div class="text" v-if="klcWeather && fnd">
        <p class="warning" v-if="weather && weather.warningMessage.length > 0">{{ ` ${weather.warningMessage[0]} ` }}

        </p>
        <p class="warning" v-else>{{ ` ` }}</p>
        <p class="forecast" v-if="fnd && fnd.weatherForecast.length > 0">{{ ` ${fnd.weatherForecast[0].forecastWeather}
          `
        }}
        </p>
        <p class="tempurature">{{ klcWeather.value }}°</p>
        <p class="maxTempurature">{{ fnd.weatherForecast[0].forecastMaxtemp.value }}°</p>
        <p class="minTempurature">{{ fnd.weatherForecast[0].forecastMintemp.value }}°</p>
        <p class="place">{{ klcWeather.place }}</p>
      </div>

      <img class="iconWeather" v-if="weather" width="12%"
        :src="`https://www.hko.gov.hk/images/HKOWxIconOutline/pic${weather.icon[0]}.png`">
    </div>

  </section>
</template>

<style scoped>
#myVideo {
  position: fixed;
  right: 0;
  bottom: 0;
  min-width: 100%;
  min-height: 100%;

  max-width: 100%;
}

h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {

  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>


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
  background-color: rgba(0, 0, 0, .3);
  font-family: 'Share Tech Mono', monospace;
  color: #ffffff;
  text-align: center;
  position: absolute;
  left: 50%;
  top: 50%;
  margin-top: 1em;
  transform: translate(-50%, -50%);
  color: #ffffff;
  text-shadow: 0 0 20px rgb(0, 167, 223), 0 0 20px rgba(10, 175, 230, 0);


  .time {
    letter-spacing: 0.05em;
    font-size: 140px;
    padding: 1px 0;
    margin-top: -0.4em;
  }

  .date {
    letter-spacing: 0.1em;
    font-size: 40px;
    margin-top: 0.5em;
  }

  .place {
    font-size: 15px;
    position: absolute;
    left: 43%;
    top: 85%;
    text-align: center;
    transform: translate(-50%, 0);
  }

  .tempurature {
    transform: translate(-40%, 0);
    position: absolute;
    left: 43%;
    top: 60%;
    text-align: center;
    font-size: 60px;
  }

  .maxTempurature {
    transform: translate(-40%, 0);
    position: absolute;
    left: 46%;
    top: 90%;
    text-align: center;
    font-size: 20px;
  }

  .minTempurature {
    transform: translate(-40%, 0);
    position: absolute;
    left: 40%;
    top: 90%;
    text-align: center;
    font-size: 20px;
  }

  .text {
    margin-top: -2.5em;
    letter-spacing: 0.1em;
    //font-size: 30px;
    padding: 2px 0;
    text-align: left;
    //padding-left: 5%;





    .warning {
      display: inline-block;
      margin-top: -0.5em;
      letter-spacing: 0.1em;
      font-size: 14px;
      padding: 0 0;
      padding-left: 5%;
      min-width: 35%;
      max-width: 35%;
      text-align: left;
      min-height: 6em;
      margin-bottom: 0.1em;
    }

    .forecast {
      display: inline-block;
      margin-top: -0.5em;
      letter-spacing: 0.1em;
      font-size: 14px;
      padding: 0 0;
      padding-left: 30%;
      max-width: 60%;
      text-align: left;
      min-height: 6em;
      margin-bottom: 0.1em;
    }
  }



  .iconWeather {
    position: absolute;
    left: 51%;
    top: 68%;
    //display: inline-block;
  }
}
</style>
