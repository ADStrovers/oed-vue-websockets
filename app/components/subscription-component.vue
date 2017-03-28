<template>
  <div>
    <ul class="channel-results" :class="channelTerm">
      <li v-for="result in tweets">
        <p class="white">{{ result.tweet.text }}</p>
      </li>
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
  computed: {
    channelTerm: function () {
      return 'channel-' + this.channel.term
    }
  },
  created() {
    this.subscribeToChannel()
  },
  beforeDestroy() {
    this.unsubscribe();
  },
  methods: {
    subscribeToChannel() {
      this.pusherChannel = this.pusher.subscribe(btoa(this.channel.term));
      this.pusherChannel.bind('new_tweet', (data) => {
        this.newTweet(data);
      });
      this.subscribed = true;
    },
    unsubscribe() {
      this.pusherChannel.unsubscribe(btoa(this.channel.term));
      this.pusherChannel && this.pusherChannel.unbind();
      this.subscribed = false;
    },
    newTweet(data) {
      this.tweets.push(data);
      this.$nextTick(() => {
        const listItem = document.querySelector(`.channel-${this.channel.term}`);
        listItem.scrollTop = listItem.scrollHeight;
      });
    }
  },
  watch: {
    'channel.active': function() {
      if (!this.channel.active && this.subscribed) {
        this.unsubscribe();
      } else if (this.channel.active && !this.subscribed) {
        this.subscribeToChannel();
      }
    }
  }
}
</script>