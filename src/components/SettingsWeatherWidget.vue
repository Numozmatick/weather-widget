<template>
  <div class="settings">
    <form @submit.prevent="getCurrentLocation">
      <div class="field">
        <label class="label">Name</label>
        <div class="control">
          <input v-model="name" class="input" type="text" />
        </div>
      </div>
    </form>
  </div>
</template>

<script>

export default {
  name: 'settings',
  props: {
    name:''
  },
  methods:{
    getCurrentLocation(){
      this.$emit('getCurrentLocation', this.name)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .weather-item{
    padding: 15px;
  }
</style>
