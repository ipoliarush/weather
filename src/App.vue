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
    <div class="box">
      <p class="sity">{{ weather.name }}, {{ country }}</p>
      <p class="date">{{ newDate }}</p>
      <div class="temp">
        <p class="temp__text">{{ Math.round(temp) }}&#176;c</p>
      </div>
      <p class="precipitation">
        {{ precipitation }}
      </p>
    </div>
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
    weather: "",
    precipitation: "",
    temp: "",
    pre: [
      { en: "broken clouds", ru: "небольшая облачность" },
      { en: "overcast clouds", ru: "пасмурно" },
    ],

    country: "",
  }),

  mounted() {
    axios
      .get(`${this.apiUrl}?q=Bucha,UA&units=metric&appid=${this.apiKey}`)
      .then((resp) => {
        this.weather = resp.data;
        this.country = resp.data.sys.country;
        this.precipitation = resp.data.weather[0].description;
        this.temp = resp.data.main.temp;
      })
      .catch((error) => console.log(error));
  },
  methods: {
    search(e) {
      if (e.key === "Enter") {
        axios
          .get(
            `${this.apiUrl}?q=${this.query}&units=metric&appid=${this.apiKey}`
          )
          .then((resp) => {
            this.weather = resp.data;
            this.country = resp.data.sys.country;
            this.precipitation = resp.data.weather[0].description;
            this.temp = resp.data.main.temp;
          })
          .catch((error) => console.log(error));

        this.query = "";
      }
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
      top: 58px;
      left: 15px;
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
          opacity: 0;
        }
      }

      &:hover &:hover {
        background: rgba(#fff, 0.7);
      }
      &:focus {
        background: rgba(#fff, 0.7);
      }
      &:focus {
        & + .label {
          opacity: 0;
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
