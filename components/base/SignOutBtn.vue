<template>
  <v-btn
    v-bind="$attrs"
    v-on="$listeners"
    @click="signOut()"
  >
    Sign Out
  </v-btn>
</template>

<script>
import { Auth } from 'aws-amplify'

export default {
  methods: {
    async signOut () {
      await Auth.signOut()
      this.$router.push('/signin')
    }
  }
}
</script>
