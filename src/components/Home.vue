<template>
  <section class="home">
    <div id="container" v-if="weather[0] != undefined">
      <div class="container-top">
        <div class="box">
          <span class="day">{{ weather[0].applicable_date }}</span>
          <img :src="imageSource[0]" alt="icon" width="80px" height="80px" />
          <span class="temperature">{{ weather[0].the_temp }}°C</span>
        </div>
        <div class="box">
          <span class="day">{{ weather[1].applicable_date }}</span>
          <img :src="imageSource[1]" alt="icon" width="80px" height="80px" />
          <span class="temperature">{{ weather[1].the_temp }}°C</span>
        </div>
        <div class="box">
          <span class="day">{{ weather[2].applicable_date }}</span>
          <img :src="imageSource[2]" alt="icon" width="80px" height="80px" />
          <span class="temperature">{{ weather[2].the_temp }}°C</span>
        </div>
        <div class="box">
          <span class="day">{{ weather[3].applicable_date }}</span>
          <img :src="imageSource[3]" alt="icon" width="80px" height="80px" />
          <span class="temperature">{{ weather[3].the_temp }}°C</span>
        </div>
        <div class="box">
          <span class="day">{{ weather[4].applicable_date }}</span>
          <img :src="imageSource[4]" alt="icon" width="80px" height="80px" />
          <span class="temperature">{{ weather[4].the_temp }}°C</span>
        </div>
      </div>
      <div class="highlights">
        <h2>Today's Highlights</h2>
        <div class="container-bottom">
          <div class="highlight-box">
            <span class="windStatus">Wind status</span>
            <p class="windData">
              <strong style="font-size: 3rem">{{ windStatus }}</strong
              >mph
            </p>
          </div>
          <div class="highlight-box">
            <span class="windStatus">Humidity</span>
            <p class="windData">
              <strong style="font-size: 3rem">{{ weather[0].humidity }}</strong>
            </p>
          </div>
          <div class="highlight-box">
            <span class="windStatus">Visibility</span>
            <p class="windData">
              <strong style="font-size: 3rem">6,4</strong> miles
            </p>
          </div>
          <div class="highlight-box">
            <span class="windStatus">Air Pressure</span>
            <p class="windData">
              <strong style="font-size: 3rem">{{
                weather[0].air_pressure
              }}</strong
              >mb
            </p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "Home",
  data() {
    return {
      imageSource: [],
      wordId: "",
      weather: {},
      windStatus: "",
      visibility: "",
    };
  },
  methods: {
    getCity(city) {
      fetch(`https://stark-garden-95720.herokuapp.com/https://www.metaweather.com/api/location/search/?query=${city}`, {
        headers: {
          "Access-Control-Allow-Origin": "*",
        },
      }).then((res) => {
        res.json().then((data) => {
          this.wordId = data[0].woeid;
          this.getWeather();
          this.imageSource = [];
        });
      });
    },
    getWeather() {
      fetch(`https://stark-garden-95720.herokuapp.com/https://www.metaweather.com/api/location/${this.wordId}/`, {
        "Access-Control-Allow-Origin": "*"
      }).then(
        (res) => {
          res.json().then(async (weather) => {
            this.weather = weather.consolidated_weather;
            for (let i = 0; i < 5; i++) {
              this.imageSource.push(
                `https://www.metaweather.com/static/img/weather/${this.weather[i].weather_state_abbr}.svg`
              );
              this.weather[i].the_temp = Math.round(this.weather[i].the_temp);
            }
            this.windStatus = Math.round(this.weather[0].wind_speed);
            this.visibility = Math.round(this.weather[0].visibility);
          });
        }
      );
    },
  },
  mounted() {
    this.$root.$refs.home = this;
  },
};
</script>

<style scoped lang="scss">
$sidebarBlue: rgb(34, 45, 77);
$primary: #11998e;
$secondary: #38ef7d;
$white: #fff;
$gray: #9b9b9b;
$dark_blue: rgb(9, 14, 28);

.home {
  background-color: $dark_blue;
  width: 100%;
  min-height: 100vh;
  font-family: "Open Sans Condensed", sans-serif;
}

#container {
  position: relative;
  width: 60%;
  height: 50%;
  margin: auto;

  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}

.container-top {
  // border: 1px solid red;

  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;

  .box {
    position: relative;
    text-align: center;
    width: 200px;
    height: 250px;
    margin-top: 20px;
    margin-right: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    color: white;
    background-color: $sidebarBlue;
  }
  .day {
    font-size: 1.5rem;
    margin-bottom: 20px;
  }
  .temperature {
    font-size: 1.5rem;
    margin-top: 20px;
  }
}

.highlights {
  text-align: center;
  color: white;
  width: 95%;
  margin-top: 50px;
}

.highlight-box {
  display: flex;
  align-items: center;
  flex-direction: column;
  flex-wrap: wrap;

  float: left;
  margin-right: 30px;
  margin-bottom: 30px;

  margin-top: 10px;
  background-color: $sidebarBlue;
  width: 350px;
  height: 200px;

  .windStatus {
    font-size: 1.5rem;
    margin-top: 10px;
  }

  .windData {
    font-size: 1.5rem;
    margin-top: 20px;
  }
}

.container-bottom {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}

@media only screen and (max-width: 1000px) {
  .highlight-box {
    width: 200px;
    height: 170px;
    margin-bottom: 20px;

    margin-right: 0px;
  }
}
</style>
