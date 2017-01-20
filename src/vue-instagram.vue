<template>
  <div>
    <slot
      name="feeds"
      v-for="feed in feeds"
      :feed="feed"
    >
    </slot>
    <p v-if="error">{{ error.error_message }}</p>
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
      (response) => {
        if (response.meta.code === 200) {
          this.profile = response.data
          this.getUserFeed()
        } else if (response.meta.code === 400) {
          this.error = response.meta
          console.log(response.meta.error_message)
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
        (response) => {
          if (response.meta.code === 200) {
            this.feeds = response.data
          } else if (response.meta.code === 400) {
            this.error = response.meta
            console.log(response.meta.error_message)
          }
        }
      )
    }
  }
}
</script>
