<template>
  <div class="weather-item">
    <div class="weather-item__location fw-semibold text-start"><span>{{ item.name }},  {{ item.country }}</span></div>
    <div class="d-flex align-items-center my-2">
      <div class="weather-item__icon me-1" :style="{backgroundImage:'url(' + item.icon + ')'}"></div>
      <div class="fs-1 fw-bold">{{ item.temp }}°</div>
    </div>
    <div class="description text-start">
      Feels like {{ item.feels_like }}°C. <span class="text-capitalize">{{ item.description }}.</span>
    </div>
    <div class="pt-3">
      <table class="">
        <tbody>
        <tr>
          <td class="param-item">
            <i class="me-1 fa-solid fa-location-arrow"></i>
            <span>{{ item.wind }} m/s SSE</span>
          </td>
          <td><i class="fa-solid fa-temperature-full me-1"></i> <span>{{item.pressure}}hPa</span></td>
        </tr>
        <tr>
          <td>Humidity: {{item.humidity}}%</td>
          <td>Dew point: {{}}</td>
        </tr>
        <tr>
          <td>Visibility: {{item.visibility}}</td>
        </tr>
        </tbody>
      </table>
    </div>

  </div>
</template>

<script>
export default {
  name: 'weather-item',
  props: {
    item: {
      type: Object
    },
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.weather-item {
  padding: 15px;
  font-size: 13px;
}

.weather-item__icon {
  width: 80px;
  height: 80px;
  background-size: contain;
}

.param-item .fa-location-arrow {
  transform: rotate(-90deg);
}

</style>
