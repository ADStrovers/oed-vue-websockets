<template>
  <div>
    <ul>
      
    </ul>
  </div>
</template>

<script>
export default {
  props: [
    'channel',
    'pusher'
  ],
  data: function () {
    return {
      tweets: []
    }
  },
  created() {
    this.subscribe();
  },
  beforeDestroy() {
    this.unsubscribe();
  },
  methods: {

  },
  watch: {

  }
}
</script>