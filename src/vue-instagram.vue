<template>
  <div :class="containerClass">
    <slot v-for="(feed, index) in feeds" name="feeds" :index="index" :feed="feed"></slot>
    <slot name="error" :error="error"></slot>
  </div>
</template>

<script>
import _ from 'lodash'
import jsonp from 'browser-jsonp'

export default {
  name: 'vue-instagram',

  props: {
    /*
    * Instagram access token.
    */
    token: {
      type: String,
      required: true
    },

    /*
    * Numbers of feed.
    */
    count: {
      type: Number,
      default: 3,
      required: false
    },

    mediaType: {
      type: String,
      required: false
    },

    tags: {
      type: Array,
      default: () => [],
      required: false
    },

    // class for container div
    containerClass: {
      type: String,
      default : '',
      required : false
    }
  },

  data: () => ({
    error: '',
    feeds: [],
    profile: ''
  }),

  mounted () {
    jsonp({
      url: 'https://api.instagram.com/v1/users/self',
      data: { access_token: this.token },
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
        url: `https://api.instagram.com/v1/users/self/media/recent`,
        data: { access_token: this.token, count: this.count },
        error: error => { throw error },
        complete: response => {
          if (response.meta.code === 400) this.error = response.meta
          if (response.meta.code === 200) {
            if (this.tags.length) {
              this.feeds = _.filter(response.data, item => _.intersection(this.tags, item.tags).length)
            } else {
              this.feeds = response.data
            }
          }
        }
      })
    }
  }
}
</script>
