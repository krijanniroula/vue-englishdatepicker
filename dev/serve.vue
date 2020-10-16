<script>
import Vue from 'vue';
import VueEnglishdatepicker from '@/vue-englishdatepicker.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VueEnglishdatepicker
  }
});
</script>

<template>
  <div id="app">
    <vue-englishdatepicker />
  </div>
</template>
