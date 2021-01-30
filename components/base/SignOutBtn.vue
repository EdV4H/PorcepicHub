<template>
  <v-btn
    @click="signOut()"
    v-bind="$attrs"
    v-on="$listeners"
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
