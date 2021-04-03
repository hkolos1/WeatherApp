<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.14.0/css/all.min.css">
    <main>

      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search ..." v-model="query" />
        <button @click="fetchWeather"><i class="fas fa-search-location"></i></button>
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ weather.main.temp }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>

      <div class="disclaimer">
        <p><i class="fas fa-exclamation-triangle"></i> After pressing the search button, it may take a while to fetch the data.</p>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      api_key: '77691bec0d0b581c91ca68e17077b6e6',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather () {
      fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res=>{
            return res.json();
          }).then(this.setResults);
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  width: 100%;
  font-family: sans-serif;
  overflow: hidden;
}
#app {
  background-image: url('https://images.unsplash.com/photo-1606669059257-19fc4ca49f79?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=968&q=80');
  background-size: cover;
  background-position: center;
  transition: .8s;
}
#app.warm {
  background-image: url('https://images.unsplash.com/photo-1606441751331-8c741ba50766?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=968&q=80');
}
main {
  min-height: 100vh;
  padding: 25px;
  background: linear-gradient(rgba(0,0,0,.1), rgba(0,0,0,.3));
}
.search-box {
  width: 100%;
  margin-bottom: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  max-width: 1000px;
  padding: 15px;
  color: #fff;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background: rgba(255,255,255,.3);
  transition: .3s;
  box-shadow: 0 2px 2px rgba(0, 0, 0, .5);
}
.search-box button {
  padding: 15px 20px;
  color: #fff;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: rgb(124,151,156);
  box-shadow: 0 2px 2px rgba(0, 0, 0, .5);
  transition: .3s;
  cursor: pointer;
}
.search-box button:hover {
  background: rgb(98, 119, 122);
}
.search-box .search-bar:focus {
  color: #333;
  background: rgba(255,255,255,.7);
  border-top-left-radius: 20px;
  border-bottom-left-radius: 20px;
  padding: 15px 25px;
}
.search-box .search-bar::placeholder {
  color: #fff;
}
.search-box .search-bar:focus::placeholder {
  color:#333;
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 2px rgba(0, 0, 0, .2);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 60px;
  font-weight: 900;
  background: rgba(255,255,255,.2);
  border-radius: 20px;
  margin: 30px 0;
  text-shadow: 1px 2px rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 16px rgba(0, 0, 0, .5);
}
.weather-box .weather {
  color: #fff;
  font-size: 45px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 1px 2px rgba(0, 0, 0, .2);
}
.disclaimer {
  position: fixed;
  display: inline-block;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  margin: 0 auto;
  background: rgba(255, 255, 255, .3);
}
.disclaimer p {
  text-align: center;
  font-size: 20px;
  padding: 15px;
  position: relative;
}
.disclaimer i {
  color: rgb(243, 70, 51);
  position: absolute;
  top: -20px;
  right: 50%;
  font-size: 30px;
  transform: translate(50%,-60%);
  animation: pop 2s infinite;
}
@keyframes pop {
  0% {
    transform:translate(50%,-60%) scale(1);
  }
  50% {
    transform:translate(50%,-60%) scale(1.2);
  }
  100% {
    transform:translate(50%,-60%) scale(1);
  }
}
</style>
