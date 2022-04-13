<template>
  <div class="weather__app">
    <form class="search__box" @submit.prevent="getWeaher">
      <label for="search">Wheather in</label>
      <input
        type="text"
        id="search"
        class="search-bar"
        placeholder="Search country ..."
        v-model="query"
        @keypress="showWeather"
      />
    </form>

    <div class="weather__content" v-if="typeof weather.main != 'undefined'">
      <h1 class="weather__content-title">{{ weather.name }}</h1>
      <p class="weather__content-temp">
        {{ Math.round(weather.main.temp) }} °C
      </p>
      <img class="weather__content-icon" src="{{icon}}" alt="" />
      <p class="weather__content-date">{{ dateBuilder }}</p>
      <!-- <p>{{ weather.weather[0].main }}</p>  -->
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
      icon: "",
    };
  },
  methods: {
    getWeaher: async function () {
      const response = await fetch(
        `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
      );
      const data = await response.json();
      this.weather = data;

      const mainWeather = this.weather.weather[0].main;

      switch (mainWeather) {
        case "Clear":
          this.icon = "./assets/images/sun.png";
          break;
        case "Clouds":
          this.icon = "./assets/images/cloud.png";
          break;
        case "Rain":
          this.icon = "./assets/images/rain.png";
          break;
        case "Snow":
          this.icon = "./assets/images/snow.png";
          break;
        case "Mist":
          this.icon = "/assets/images/mist.png";
          break;
        default:
          console.log("Juma yoki Shanba deb yoz");
      }

      console.log(this.icon);
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
#app {
  display: flex;
  align-items: center;
  justify-content: center;
  background-image: url(assets/images/image.png);
  background-repeat: no-repeat;
  background-size: cover;
  height: 100vh;
}
.weather__app {
  width: 600px;
  padding: 62px;
  background: linear-gradient(
    180deg,
    rgba(244, 249, 253, 0.35) 0%,
    rgba(245, 245, 247, 0.35) 100%
  );
  border: 3px solid rgba(255, 255, 255, 0.6);
  backdrop-filter: blur(77.8321px);
  border-radius: 80px;
}

.search__box {
  margin-bottom: 60px;
}

.search__box label {
  font-size: 35px;
  font-weight: 700;
  color: #000;
  text-align: center;
  display: block;
  margin-bottom: 25px;
}

.search__box input {
  width: 100%;
  height: 100%;
  border: none;
  border-radius: 20px;
  border: 2px solid rgba(255, 255, 255, 0.6);
  padding: 20px;
  font-size: 20px;
  font-weight: bold;
  background: linear-gradient(
    180deg,
    rgba(244, 249, 253, 0.35) 0%,
    rgba(245, 245, 247, 0.35) 100%
  );
  backdrop-filter: blur(77.8321px);
  color: #393838;
  outline: 0;
}

.search__box input::placeholder {
  color: #393838;
}

.weather__content {
  text-align: center;
  border-radius: 20px;
  padding: 30px 20px;
  background: linear-gradient(
    180deg,
    rgba(188, 186, 253, 0.52) 0%,
    #71bff0 100%
  );
  box-shadow: -7.78321px -7.78321px 31.1329px rgba(255, 255, 255, 0.85),
    7.78321px 7.78321px 31.1329px -3.11329px rgba(91, 115, 134, 0.78),
    inset 12.4531px 14.0098px 14.0098px rgba(255, 255, 255, 0.33),
    inset -9.33986px -10.8965px 12.4531px rgba(0, 0, 0, 0.05);
}

.weather__content-title {
  color: #fff;
  font-weight: 700;
  margin-bottom: 20px;
}

.weather__content-date {
  color: #fff;
  font-weight: 400;
  font-size: 25px;
  margin-bottom: 20px;
}

.weather__content-temp {
  font-weight: 700;
  font-size: 40px;
  color: #f8f8f8;
  text-shadow: 0px 3.11329px 6.22657px rgba(153, 178, 240, 0.74);
}

.weather__content-icon {
  display: block;
  max-width: 200px;
  max-height: 150px;
  margin: 20px auto;
}
</style>
