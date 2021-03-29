<template>
  <section class="sidebar">
    <aside>
      <div class="input-container">
        <input
          type="text"
          class="city_input"
          name="city"
          v-model="city"
          placeholder="City"
          required
        />
        <button type="submit" class="searchButton" @click.prevent="getCity()">
          Search
        </button>
      </div>
      <div class="clouds">
        <img
          src="~@/assets/cloud.png"
          alt=""
          width="50%"
          class="cloud_image firstCloud"
        />
        <img
          src="~@/assets/cloud.png"
          alt=""
          width="50%"
          class="cloud_image secondCloud"
          style="float: right"
        />
      </div>
      <div class="weatherDescription" v-if="weather[0] != undefined">
        <img
          :src="imageSource"
          alt="weatherIcon"
          width="50%"
          class="weatherIcon"
        />
        <h2 class="temperature">{{ temp }}°C</h2>
        <p style="font-size: 2rem; color: white; margin-top: 50px">{{ weatherStateName }}</p>
      </div>
    </aside>
  </section>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "Sidebar",
  data() {
    return {
      city: "",
      wordId: "",
      weather: {},
      weatherStateName: "",
      temp: "",
      imageSource: ""
    };
  },
  methods: {
    getCity() {
      this.$root.$refs.home.getCity(this.city);

      fetch(
        `https://stark-garden-95720.herokuapp.com/https://www.metaweather.com/api/location/search/?query=${this.city}`
      ).then((res) => {
        res.json().then((data) => {
          this.wordId = data[0].woeid;
          this.getWeather();
        });
      });
    },
    getWeather() {
      fetch(`https://stark-garden-95720.herokuapp.com/https://www.metaweather.com/api/location/${this.wordId}/`).then(
        (res) => {
          res.json().then((weather) => {
            this.weather = weather.consolidated_weather;
            this.weatherStateName = this.weather[0].weather_state_name;
            this.temp = Math.round(this.weather[0].the_temp);
            this.imageSource = `https://www.metaweather.com/static/img/weather/${this.weather[0].weather_state_abbr}.svg`;
          });
        }
      );
    },
  },
};
</script>

<style scoped lang="scss">
$sidebarBlue: rgb(34, 45, 77);
$buttonBlue: #134676;
$primary: #11998e;
$secondary: #38ef7d;
$white: #fff;
$gray: #9b9b9b;

.sidebar {
  width: 350px;
  height: 100vh;
  background-color: $sidebarBlue;
  font-family: "Open Sans Condensed", sans-serif;
  overflow: hidden;
}

@media only screen and (max-width: 1000px) {
  .sidebar {
    width: 100%;
  }
}

.clouds {
  position: relative;
}

.temperature {
  color: white;
  font-size: 5rem;
  top: 50%;
}

.weatherDescription {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-top: 50%;
}

.city_input {
  margin-top: 25px;
  padding: 10px;
  outline: none;
  border: none;
  border-bottom: 1px solid $gray;
  color: white;
  background-color: transparent;
}

.searchButton {
  outline: none;
  border: none;
  background-color: $buttonBlue;
  color: white;
  padding: 15px;
  cursor: pointer;
  margin-top: 10px;
}

.input-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.cloud_image {
  margin-top: 20px;
  width: 60%;
}
.firstCloud {
  position: absolute;
  //animation: clouds-animate 11s linear 1s infinite;
  left: -30%;
  animation: move 5s linear 5s infinite;
}
.secondCloud {
  position: absolute;
  margin-top: 60%;
  //animation: clouds-animate 11s linear 1s infinite;
  left: -30%;
  animation: move2 5s linear 5s infinite;
}

@-webkit-keyframes move {
  from {
    left: -60%;
  }
  to {
    left: 100%;
  }
}

@-webkit-keyframes move2 {
  from {
    left: 100%;
  }
  to {
    left: -60%;
  }
}
</style>
