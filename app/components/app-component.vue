<template>

</template>

<script>
import Pusher from 'pusher-js';
import SubscriptionComponent from './subscription-component.vue';

export default {
  data: function () {
    return {
      newSearchTerm: '',
      channels: []
    }
  },
  components: {
    'subscription-component': SubscriptionComponent
  },
  created() {

  },
  methods: {

  }
}
</script>

<style>

</style>