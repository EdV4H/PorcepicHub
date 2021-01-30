<template>
  <div>
    account page
  </div>
</template>

<script>
import { Auth } from 'aws-amplify'

export default {
  mounted () {
    this.getAccount()
  },
  methods: {
    async getAccount () {
      const userInfo = await Auth.currentUserInfo()
      console.log(userInfo)
    }
  }
}
</script>
