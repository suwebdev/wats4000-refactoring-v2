<template>
  <div>
    <h2>City Search</h2>
    <form v-on:submit.prevent="getCities">
        <p>Enter city name: <input type="text" v-model="query" placeholder="Paris"> <button type="submit">Go</button></p>
    </form>
    <ul class="cities" v-if="results && results.list.length > 0">
        <li v-for="(city,index) in results.list" :key="index">
            <h2>{{ city.name }}, {{ city.sys.country }}</h2>
            <p><router-link v-bind:to="{ name: 'CurrentWeather', params: { cityId: city.id } }">View Current Weather</router-link></p>

            <!-- TODO: Make weather summary be in a child component. -->
            <div v-for="(weatherSummary,index) in city.weather" class="weatherSummary" :key="index">
                <img v-bind:src="'http://openweathermap.org/img/w/' + weatherSummary.icon + '.png'" v-bind:alt="weatherSummary.main">
                <br>
                <b>{{ weatherSummary.main }}</b>
            </div>
            <!-- TODO: Make dl of weather data be in a child component. -->
            <dl>
                <dt>Current Temp</dt>
                <dd>{{ city.main.temp }}&deg;F</dd>
                <dt>Humidity</dt>
                <dd>{{ city.main.humidity }}%</dd>
                <dt>High</dt>
                <dd>{{ city.main.temp_max }}&deg;F</dd>
                <dt>Low</dt>
                <dd>{{ city.main.temp_min }}&deg;F</dd>
            </dl>
        </li>
    </ul>
    <div v-else-if="errors.length > 0">
      <h2>There was an error fetching weather data.</h2>
      <ul class="errors">
        <li v-for="(error,index) in errors" :key="index">{{ error }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'CitySearch',
  data () {
    return {
      results: null,
      errors: [],
      query: ''
    }
  },
  methods: {
    getCities: function () {
      // TODO: Improve base config for API
      axios.get('//api.openweathermap.org/data/2.5/find', {
        params: {
            q: this.query,
            units: 'imperial',
            APPID: 'YOUR_APPID_HERE'
        }
      })
      .then(response => {
        this.results = response.data
      })
      .catch(error => {
        this.errors.push(error)
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.errors li {
  color: red;
  border: solid red 1px;
  padding: 5px;
}
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  width: 300px;
  min-height: 300px;
  border: solid 1px #e8e8e8;
  padding: 10px;
  margin: 5px;
}
.weatherSummary {
  display: inline-block;
  width: 100px;
}
dl {
  padding: 5px;
  background: #e8e8e8;
}
dt {
  float: left;
  clear: left;
  width: 120px;
  text-align: right;
  font-weight: bold;
  color: blue;
}
dd {
  margin: 0 0 0 130px;
  padding: 0 0 0.5em 0;
}
dt::after {
  content: ":";
}

a {
  color: #42b983;
}
</style>


