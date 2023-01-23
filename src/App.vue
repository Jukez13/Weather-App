<template>
  <div class="id">
    <div>
      <h1>🌤️Vue-Weather</h1>
      <input type="text" class="sbar" placeholder="Search Location" v-model="search" @keypress="fetchWeather" /> &nbsp;
      <button type="submit" class="srch" @click="submit">Search</button>
      <div class="head" v-if="typeof weather.main != 'undefined'">
        <div class="loc">
          <span>{{ weather.name }} , {{ weather.sys.country }}</span>
        </div>
        <div class="date">
          <span>{{ timestamp() }}</span>
        </div>
      </div>
      <div class="error" v-else-if="typeof weather.main == 'undefined'">
        <h1>Enter The City Name... </h1>
      </div>
    </div>
    <div class="box" v-if="typeof weather.main != 'undefined'">
      <div>{{ Math.round(weather.main.temp) }}°c</div>
      <div>{{ weather.weather[0].main }}</div>
    </div>

    <div class="flexrow" v-if="typeof weather.main != 'undefined'">
      <div class="flexcolumn">
        <br />
        <span>Latitude: {{ weather.coord.lat }}</span> <br />
        <span>Longitude:{{ weather.coord.lon }}</span> <br />
        <span>Feels Like:{{ weather.main.feels_like }}°c</span> <br />
        <span>Humidity:{{ weather.main.humidity }}%</span> <br />
      </div>
      <div class="flexcolumn">
        <br />
        <span>Pressure:{{ weather.main.pressure }}mbar</span><br />
        <span>Weather Description:{{ weather.weather[0].description }}</span><br />
        <span>Wind Degree:{{ weather.wind.deg }}</span><br />
        <span>Wind Speed:{{ weather.wind.speed }}Km/h</span><br />
      </div>
    </div>


  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      api_key: 'c6bd1eb220f716b4c1b3183cd6cf455b',
      base_url: 'https://api.openweathermap.org/data/2.5/',
      search: '',
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.base_url}weather?q=${this.search}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    submit() {
      fetch(`${this.base_url}weather?q=${this.search}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
    },
    setResults(results) {
      this.weather = results;
      console.log(this.weather)
      console.log(typeof this.weather.main)
    },
    timestamp() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
html {
  background: url('./bg-img.jpg');
  background-size: cover;
}

.loc {
  color: black;
  font-weight: bolder;
  font-size: 45px;
  text-align: right;
  font-style: normal;
  margin-top: -90px;
  margin-right: 80px;
}

.date {
  font-style: italic;
  font-size: large;
  text-align: right;
  color: black;
  margin-top: 0px;
  margin-right: 80px;
}


.head {
  text-align: end;
}

.box {
  display: flexbox;
  box-sizing: content-box;
  margin-top: 40px;
  margin-bottom: 20px;
  padding: 10px;
  border: 3px solid white;
  text-align: center;
  font-weight: bolder;
  font-size: 40px;
  backdrop-filter: blur(5.1px);
  -webkit-backdrop-filter: blur(5.1px);
}

.error {
  color: red;
  text-align: center;
}

.flexrow {
  display: flex;
  justify-content: space-evenly;
  width: 100%;
  align-items: center;
  backdrop-filter: blur(5.1px);
  -webkit-backdrop-filter: blur(5.1px);
}

.flexcolumn {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 80%;
  font-size: 20px;
  font-weight: bold;
  color: darkblue;
  backdrop-filter: blur(5.1px);
  -webkit-backdrop-filter: blur(5.1px);
}
</style>