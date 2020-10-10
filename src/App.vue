<template>
  <div id="app" class="weather">
    <div class="form">
      <form action="#" @submit.prevent="">
        <div class="group">
          <input
            id="input"
            class="input"
            type="text"
            spellcheck="false"
            autocomplete="off"
            placeholder=" "
            v-model="query"
            @keypress="search"
          />
          <label class="label" for="input">Город</label>
        </div>
      </form>
    </div>
    <transition name="fade" mode="out-in">
      <div v-if="weather" v-cloak class="box">
        <p class="sity">{{ weather.name }}, {{ weather.sys.country }}</p>
        <p class="date">{{ newDate }}</p>
        <div class="temp">
          <p class="temp__text">{{ Math.round(weather.main.temp) }}&#176;c</p>
        </div>
        <p class="precipitation">
          {{ weather.weather[0].description }}
        </p>
      </div>
      <div v-else class="box">
        <p class="date">
          Выберите город или разрешите определение вашей геолокации
        </p>
      </div>
    </transition>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data: () => ({
    apiKey: "a6615e60438fe4856be0e8b1bb030fdd",
    apiUrl: "https:/api.openweathermap.org/data/2.5/weather",
    query: "",
    weather: null,
    precipitation: "",
    temp: "",
    newThis: "",
    pre: [
      { en: "broken clouds", ru: "небольшая облачность" },
      { en: "overcast clouds", ru: "пасмурно" },
      { en: "light rain", ru: "легкий дождь" },
    ],

    country: "",
  }),

  mounted() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition((pos) => {
        this.getGeolocation(pos.coords.latitude, pos.coords.longitude);
      });
    }
  },
  methods: {
    search(e) {
      if (e.key === "Enter") {
        this.getMeteoByCity(this.apiUrl, this.query, this.apiKey)
          .then((resp) => (this.weather = resp.data))
          .catch((error) => console.log(error));
        this.query = "";
      }
    },
    getGeolocation(lat, lon) {
      this.getMeteoByCoords(this.apiUrl, lat, lon, this.apiKey)
        .then((resp) => (this.weather = resp.data))
        .catch((error) => console.log(error));
    },
    getMeteoByCity(url, city, key) {
      return axios.get(`${url}?q=${city}&units=metric&appid=${key}`);
    },
    getMeteoByCoords(url, lat, lon, key) {
      return axios.get(
        `${url}?units=metric&lat=${lat}&lon=${lon}&appid=${key}`
      );
    },
  },
  computed: {
    newDate: function () {
      let d = new Date();
      let months = [
        "января",
        "февраля",
        "марта",
        "апреля",
        "мая",
        "июня",
        "июля",
        "августа",
        "сентября",
        "октября",
        "ноября",
        "декабря",
      ];
      let days = [
        "Воскресенье",
        "Понедельник",
        "Вторник",
        "Среда",
        "Четверг",
        "Пятница",
        "Суббота",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },
  update() {},
};
</script>

<style lang="scss" scoped>
.weather {
  font-family: "Montserrat", sans-serif;
  background: #8d405a;
  width: 100%;
  min-height: 100vh;
  font-weight: 500;
  color: #fff;

  .form {
    max-width: 400px;
    width: 100%;
    margin-right: auto;
    margin-left: auto;

    .group {
      position: relative;
      padding-top: 50px;
      font-weight: 400;
      color: #1a1a1a;
    }
    .label {
      position: absolute;
      top: 0;
      left: 0;
      transform: translate(15px, 58px);
      font-weight: 500;
      color: #fff;
      transition: 0.5s;
    }
    .input {
      background: rgba(#fff, 0.4);
      border: none;
      border-radius: 0 16px 0 16px;
      padding: 8px 15px;
      width: 100%;
      box-shadow: 5px 5px 5px rgba(#000, 0.25);
      font-weight: 500;
      color: #fff;
      transition: 0.5s;

      &:not(:placeholder-shown) {
        & + .label {
          transform: translate(15px, 25px);
        }
      }

      &:hover {
        background: rgba(#fff, 0.7);
      }
      &:focus {
        background: rgba(#fff, 0.7);
      }
      &:focus {
        & + .label {
          transform: translate(15px, 25px);
        }
      }
    }
  }

  .box {
    max-width: 400px;
    width: 100%;
    margin-right: auto;
    margin-left: auto;
    margin-top: 30px;
    text-align: center;

    .sity {
      font-size: 35px;
      font-weight: 600;
      text-shadow: 1px 3px rgba(#000, 0.25);
    }
    .date {
      font-size: 20px;
      font-weight: 300;
      font-style: italic;
      margin-bottom: 20px;
      color: rgb(233, 233, 233);
    }

    .temp {
      background: rgba(#fff, 0.25);
      width: 250px;
      margin: 10px auto;
      height: 170px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 102px;
      font-weight: 900;
      border-radius: 16px;
      text-shadow: 3px 6px rgba(#000, 0.25);
      margin-bottom: 20px;
    }
    .precipitation {
      font-size: 40px;
      font-weight: 800;
      text-shadow: 1px 3px rgba(#000, 0.25);
      font-style: italic;
    }
  }
}
</style>
