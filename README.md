# vue-instagram

[![npm version](https://badge.fury.io/js/vue-instagram.svg)](https://badge.fury.io/js/vue-instagram)
[![npm](https://img.shields.io/npm/dt/vue-instagram.svg)](https://www.npmjs.com/package/vue-instagram)
[![npm](https://img.shields.io/npm/l/vue-instagram.svg)](http://opensource.org/licenses/MIT)

Fetch Instagram feeds component based on [Vue](https://vuejs.org/) 2.

## Demo

[My Instagram's feed] (https://kevinongko.github.io/vue-instagram/)

## Installation

```sh
$ npm install vue-instagram --save
```


## Usage

Style your feeds using [scoped slot] (https://vuejs.org/v2/guide/components.html#Scoped-Slots)

```vue
<template>
  <vue-instagram token="accessTokenHere" username="kevinongko" :count="5">
    <template slot="feeds" scope="props">
      <li class="fancy-list"> {{ props.feed.link }} </li>
    </template>
    <template slot="error" scope="props">
      <div class="fancy-alert"> {{ props.error.error_message }} </div>
    </template>
  </vue-instagram>
</template>

<script>
import VueInstagram from 'vue-instagram'

export default {
  name: 'App',

  components: {
    VueInstagram
  }
}
</script>

```

## Props
```js
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
    default: 3
    required: false,
    type: Number
  },
```

## License

Vue-Instagram is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)
