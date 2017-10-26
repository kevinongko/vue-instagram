<template>
  <div>
    <slot v-for="(feed, index) in feeds" name="feeds" :feed="feed"></slot>
    <slot name="error" :error="error"></slot>
  </div>
</template>

<script>
import jsonp from 'browser-jsonp'

export default {
  name: 'vue-instagram',

  props: {
    /*
    * Instagram access token.
    */
    token: {
      required: true,
      type: String
    },

    /*
    * Instagram username.
    */
    username: {
      required: true,
      type: String
    },

    /*
    * Numbers of feed.
    */
    count: {
      default: 3,
      required: false,
      type: Number
    }
  },

  data: () => ({
    error: '',
    feeds: [],
    profile: ''
  }),

  mounted () {
    jsonp({
      url: 'https://api.instagram.com/v1/users/search',
      data: { access_token: this.token, q: this.username },
      error: error => { throw error },
      complete: response => {
        if (response.meta.code === 400) this.error = response.meta
        if (response.meta.code === 200) {
          this.profile = response.data
          this.getUserFeed()
        }
      }
    })
  },

  methods: {
    getUserFeed () {
      jsonp({
        url: `https://api.instagram.com/v1/users/${this.profile[0].id}/media/recent`,
        data: { access_token: this.token, count: this.count },
        error: error => { throw error },
        complete: response => {
          if (response.meta.code === 400) this.error = response.meta
          if (response.meta.code === 200) this.feeds = response.data
        }
      })
    }
  }
}
</script>
