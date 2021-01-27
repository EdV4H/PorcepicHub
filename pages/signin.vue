<template>
  <a href="#" @click="signIn('Google')">Googleでログイン</a>
</template>

<script>
import { Auth } from 'aws-amplify'

export default {
  methods: {
    async signIn (provider) {
      await Auth.federatedSignIn({ provider })
    }
  }
}
</script>
