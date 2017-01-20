<template>
  <div>
    <slot
      name="feeds"
      v-for="feed in feeds"
      :feed="feed"
    >
    </slot>
    <slot name="error" :error="error"></slot>
  </div>
</template>

<script>
import jsonp from 'jsonp'

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
    profile: '',
    feeds: [],
    error: ''
  }),

  mounted () {
    jsonp('https://api.instagram.com/v1/users/search?access_token=' + this.token + '&q=' + this.username, null,
      (err, data) => {
        if (err) {
          console.log(err)
        } else if (data.meta.code === 400) {
          this.error = data.meta
          console.log(data.meta.error_message)
        } else if (data.meta.code === 200) {
          this.profile = data.data
          this.getUserFeed()
        }
      }
    )
  },

  methods: {
    getUserFeed () {
      jsonp(
        'https://api.instagram.com/v1/users/' + this.profile[0].id +
        '/media/recent?access_token=' + this.token + '&count=' + this.count,
        null,
        (err, data) => {
          if (err) {
            console.log(err)
          } else if (data.meta.code === 400) {
            this.error = data.meta
            console.log(data.meta.error_message)
          } else if (data.meta.code === 200) {
            this.feeds = data.data
          }
        }
      )
    }
  }
}
</script>
