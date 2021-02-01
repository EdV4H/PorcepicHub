<template>
  <div>
    <v-card
      max-width="475"
      class="mx-auto"
    >
      <v-list
        two-line
        subheader
      >
        <v-subheader>アカウント情報</v-subheader>

        <v-list-item>
          <v-list-item-content>
            <v-list-item-title>ユーザーID</v-list-item-title>
            <v-list-item-subtitle>{{ id }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <v-divider />

      <v-list
        subheader
        flat
      >
        <v-subheader>アカウント設定</v-subheader>

        <v-list-item @click="signOut()">
          <v-list-item-icon>
            <v-icon>
              mdi-logout
            </v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>ログアウト</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-card>
  </div>
</template>

<script>
import { Auth } from 'aws-amplify'

export default {
  middleware: 'auth',
  data () {
    return {
      id: undefined
    }
  },
  mounted () {
    this.getAccount()
  },
  methods: {
    async getAccount () {
      const authInfo = await Auth.currentUserInfo()
      this.id = authInfo.username
    },
    async signOut () {
      await Auth.signOut()
      this.$router.push('/signin')
    }
  }
}
</script>
