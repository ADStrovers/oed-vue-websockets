<template>
  <div>
    <div id="search-form">
      <form v-on:submit.prevent="newSubscription">
        <input class="swish-input" v-model="newSearchTerm" placeholder="JavaScript" />
        <button class="bright-blue-hover btn-white">Search</button>
      </form>
    </div>
    <div class="container tweets-container">
      <div id="channels-list">
        <div class="channel" v-for="channel in channels">
          <h3>
            Tweets for {{ channel.term }}
          </h3>
          <div id="subscription-controls">
            <button v-on:click.prevent="toggleSearch(channel)">
              {{channel.active ? 'Stop' : 'Restart'}} Stream
            </button>
            <button v-on:click.prevent="clearSearch(channel)">
              Remove Results
            </button>
          </div>
          <subscription-component
            :channel="channel"
            :pusher="pusher"></subscription-component>
        </div>
      </div>
    </div>
  </div>
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
    this.pusher = new Pusher('85d7e982ffd04c44f323');
  },
  methods: {
    newSubscription() {
      this.channels.push({
        term: this.newSearchTerm,
        termClass: 'channel-' + this.newSearchTerm,
        active: true
      });
      this.newSearchTerm = '';
    },
    toggleSearch(channel) {
      for (let ch of this.channels) {
        if (ch.term === channel.term) {
          ch.active = !ch.active;
          break;
        }
      }
    },
    clearSearch(channel) {
      this.channels = this.channels.filter((ch) => {
        return ch.term !== channel.term;
      });
    }
  }
}
</script>

<style>
#search-form {
  margin-top: 30px;
}

.channel {
  margin-bottom: 50px;
}

.channel-results {
  list-style: none;
  padding: 0;
  overflow: scroll;
  max-height: 400px;
}

#subscription-controls {
  margin-top: 20px;
  margin-bottom: 20px;
}

#subscription-controls button {
  padding: 10px 15px;
}

.twitter-icon {
  vertical-align: top;
}

.tweets-container {
  margin: auto;
  width: 800px;
}
</style>