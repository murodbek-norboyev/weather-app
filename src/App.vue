<template>
  <div class="container" :class="isDay ? 'day' : 'night'">
    <div class="weather__app">
      <form class="search__box" @submit.prevent="getWeaher">
        <label for="search">Wheather in</label>
        <input
            type="text"
            id="search"
            class="search-bar"
            placeholder="Search city ..."
            v-model="query"
            @keypress="showWeather"
            autocomplete="off"
        />
      </form>


      <div class="weather__content" v-if="cityFound">
        <h1 class="weather__content-title">{{ weather.name }}</h1>
        <p class="weather__content-date">{{ dateBuilder }}</p>
        <p class="weather__content-temp">
          {{ Math.round(weather.main.temp) }} °C
        </p>
        <img :src="icon" :alt="weather.name" class="weather__content-icon">
        <p class="weather__content-date">{{ weather.weather[0].main }}</p>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "6c33b9b1e061ff1e1c91baef1d8fe885",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      cityFound: false,
      isDay: false,
      icons: {
        day: {
          sun: require('@/assets/images/sun.png'),
          cloud: require('@/assets/images/dcloud.png'),
          rain: require('@/assets/images/drain.png'),
          snow: require('@/assets/images/dsnow.png'),
          storm: require('@/assets/images/dstorm.png'),
          mist: require('@/assets/images/dwind.png')
        },
        night: {
          moon: require('@/assets/images/moon.png'),
          cloud: require('@/assets/images/ncloud.png'),
          rain: require('@/assets/images/nrain.png'),
          snow: require('@/assets/images/nsnow.png'),
          storm: require('@/assets/images/nstorm.png'),
          mist: require('@/assets/images/nwind.png')
        }
      }
    };
  },
  methods: {
    getWeaher: async function () {

      const baseURL = `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`;

      fetch(baseURL)
          .then(async (data) => {
            if (data.ok) {
              data = await data.json()
              this.weather = data;
              this.cityFound = true;

              const timeOfDay = data.weather[0].icon;

              if (timeOfDay.includes("n")) {
                this.isDay = false;
              } else {
                this.isDay = true;
              }
            } else {
              this.cityFound = false;
            }
          }).catch(e => {
        this.cityFound = false;
        console.log('Connection error', e)
      })
    },
  },
  computed: {
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
    icon() {
      if (this.cityFound) {
        if (this.isDay) {
          if (this.weather.weather[0].main == "Clear") {
            return this.icons.day.sun
          } else if (this.weather.weather[0].main == "Clouds") {
            return this.icons.day.cloud
          } else if (this.weather.weather[0].main == "Rain") {
            return this.icons.day.rain
          } else if (this.weather.weather[0].main == "Snow") {
            return this.icons.day.snow
          } else if (this.weather.weather[0].main == "Thunderstorm") {
            return this.icons.day.storm
          } else if (this.weather.weather[0].main == "Haze") {
            return this.icons.day.mist
          }
        } else {
          if (this.weather.weather[0].main == "Clear") {
            return this.icons.night.sun
          } else if (this.weather.weather[0].main == "Clouds") {
            return this.icons.night.cloud
          } else if (this.weather.weather[0].main == "Rain") {
            return this.icons.night.rain
          } else if (this.weather.weather[0].main == "Snow") {
            return this.icons.night.snow
          } else if (this.weather.weather[0].main == "Thunderstorm") {
            return this.icons.night.storm
          } else if (this.weather.weather[0].main == "Haze") {
            return this.icons.night.mist
          }
        }
        return console.log('worked')
      } else {
        return console.log('not worked')
      }
    }
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: "Lato", sans-serif;
}

.container {
  display: flex;
  align-items: center;
  justify-content: center;
  background-repeat: no-repeat;
  background-size: cover;
  height: 100%;
  min-height: 100vh;
  background-image: url(assets/images/day.png);
}

.weather__app {
  max-width: 550px;
  width: calc(100% - 50px);
  padding: 35px 30px;
  background: linear-gradient(
          180deg,
          rgba(244, 249, 253, 0.35) 0%,
          rgba(245, 245, 247, 0.35) 100%
  );
  border: 3px solid rgba(255, 255, 255, 0.6);
  backdrop-filter: blur(77.8321px);
  border-radius: 30px;

  @media(min-width: 576px) {
    padding: 45px 62px;
    border-radius: 80px;
  }
}

.search__box label {
  font-size: 28px;
  font-weight: 700;
  color: #000;
  text-align: center;
  display: block;
  margin-bottom: 25px;

  @media(min-width: 576px) {
    font-size: 35px;
  }
}

.search__box input {
  width: 100%;
  height: 100%;
  border-radius: 20px;
  border: 2px solid rgba(255, 255, 255, 0.6);
  padding: 15px 10px;
  font-size: 16px;
  font-weight: bold;
  background: linear-gradient(
          180deg,
          rgba(244, 249, 253, 0.35) 0%,
          rgba(245, 245, 247, 0.35) 100%
  );
  backdrop-filter: blur(77.8321px);
  color: #393838;
  outline: 0;

  @media(min-width: 576px) {
    font-size: 20px;
  }
}

.search__box input::placeholder {
  color: #393838;
}

.weather__content {
  margin-top: 40px;
  text-align: center;
  border-radius: 20px;
  padding: 25px 20px;
  background: linear-gradient(
          180deg,
          rgba(188, 186, 253, 0.52) 0%,
          #71bff0 100%
  );
  box-shadow: -7.78321px -7.78321px 31.1329px rgba(255, 255, 255, 0.85),
  7.78321px 7.78321px 31.1329px -3.11329px rgba(91, 115, 134, 0.78),
  inset 12.4531px 14.0098px 14.0098px rgba(255, 255, 255, 0.33),
  inset -9.33986px -10.8965px 12.4531px rgba(0, 0, 0, 0.05);

  @media(min-width: 576px) {
    margin-top: 60px;
  }
}

.weather__content-title {
  color: #fff;
  font-weight: 700;
  margin-bottom: 20px;
  font-size: 25px;

  @media(min-width: 576px) {
    font-size: 30px;
  }
}

.weather__content-date {
  color: #fff;
  font-weight: 400;
  font-size: 20px;
  margin-bottom: 0;
  @media(min-width: 576px) {
    font-size: 25px;
  }
}

.weather__content-temp {
  font-weight: 700;
  font-size: 35px;
  color: #f8f8f8;
  text-shadow: 0px 3.11329px 6.22657px rgba(153, 178, 240, 0.74);
  margin-top: 25px;

  @media(min-width: 576px) {
    font-size: 40px;
  }
}

.weather__content-icon {
  display: block;
  max-width: 130px;
  max-height: 150px;
  margin: 20px auto;

  @media(min-width: 576px) {
    max-width: 200px;
    max-height: 150px;
  }
}
</style>
