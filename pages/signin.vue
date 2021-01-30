<template>
  <div>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="8" md="6">
        <v-card class="mb-3">
          <v-card-text>
            <v-img
              :src="require('~/static/icon.png')"
              max-height="150"
              contain
            />
          </v-card-text>
          <v-card-title class="headline">
            Welcome to the Porcepic Hub
          </v-card-title>
          <v-card-text>
            <p>Porcepic Hubは飲み会をちょっと楽しくするために作られたWebアプリケーションです．</p>
            <p>下のボタンからログインして今すぐ始めましょう．</p>
            <div class="text-xs-right">
              <em><small>&mdash; Yusuke Maruyama</small></em>
            </div>
          </v-card-text>
        </v-card>
        <base-google-btn
          class="mb-3"
          block
          @click="signIn('Google')"
        />
        <v-btn
          large
          block
          color="green"
          @click="signIn('LINE')"
        >
          <v-icon
            left
            large
          >
            mdi-alpha-l
          </v-icon>
          Sign in with LINE
        </v-btn>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import { Auth } from 'aws-amplify'

export default {
  methods: {
    async signIn (provider) {
      if (location.origin === 'http://localhost:3000') {
        // debug
        await Auth.signIn('admin', 'phubadmin')
        this.$router.push('/')
      } else {
        await Auth.federatedSignIn({ provider })
      }
    }
  }
}
</script>
