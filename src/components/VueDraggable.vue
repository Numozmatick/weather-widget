<template>
  <div>
    <div class="position-relative w-100">
      <div class="position-absolute top-0 end-0">
        <i v-if="!setting" class="cursor-pointer fa-solid fs-4 fa-gear pe-4" @click="setting = true"></i>
        <i v-else class="cursor-pointer fa-solid fs-4 fa-xmark pe-4" @click="setting = false"></i>
      </div>
    </div>
    <draggable
        v-model="locations"
        group="locations"
        @start="drag=true"
        @end="drag=false"
        item-key="id">
      <template #item="{element}">
        <div>{{element.name}}</div>
      </template>
    </draggable>
    <div v-show="!setting" v-for="item in items" :key="item.name" v-cloak>
      <weather-item :item="item"/>
    </div>
    <settings-weather-widget v-show="setting" @getInputLocation="addNewLocationFromInput"/>
  </div>

</template>
<script src="//cdn.jsdelivr.net/npm/sortablejs@1.10.2/Sortable.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/Vue.Draggable/4.0.0/vuedraggable.umd.min.js"></script>
<script>
import WeatherItem from './WeatherItem.vue'
import SettingsWeatherWidget from './SettingsWeatherWidget.vue'
export default {
  name: 'weather-widget',
  props: {
    msg: String
  },
  data: function () {
    return {
      url: 'https://api.openweathermap.org/data/2.5/weather',
      appid: 'b9a82caab604c5d354482f823fef41bb',
      urlForIcon: 'http://openweathermap.org/img/wn/',
      locations:['Moscow'],
      items:[],
      setting:false
    }
  },
  mounted() {
    this.locations = JSON.parse(localStorage.getItem('locations')) || [];
    this.locations.map(l => this.addNewLocation(l))
  },

  methods: {
    getDataFromOpenWeather: async function (location, appid) {
      return  fetch(`${this.url}?q=${location}&appid=${appid}&units=metric`).then(function (response) {
        return response.json();
      }).then(function (data) {
        return data
      }).catch(function () {
        console.log("Error");
      });
    },

    generateIconUrl: function (code) {
      return this.urlForIcon + `${code}.png`
    },

    removeLocation: function (location){
      const locations = JSON.parse(localStorage.getItem('locations'));
      const result = locations.filter((l) => (l !== location))

      localStorage.setItem('locations', JSON.stringify(result));
      this.locations = result;
    },

    addNewLocationFromInput:function (location){
      if(!this.locations.includes(location)) {
        this.addNewLocation(location)
      }
    },

    addNewLocation:async function (location){
      if(!this.locations.includes(location)) {
        this.locations.push(location)
        localStorage.setItem('locations', JSON.stringify(this.locations));
      }
      await this.createNewItem(location)
    },


    createNewItem: async function (location){
      const data = await this.getDataFromOpenWeather(location, this.appid);
      const item = {
        name: data.name,
        country: data.sys && data.sys.country,
        icon: this.generateIconUrl(data.weather[0].icon),
        temp: data.main.temp,
        wind: data.wind.speed,
        description: data.weather[0].description,
        feels_like: data.main.feels_like,
        humidity: data.main && data.main.humidity,
        pressure: data.main && data.main.pressure,
        visibility: data.visibility,
      }
      this.items.push(item)
    }
  },
  components: {
    WeatherItem,
    SettingsWeatherWidget,
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
