<template>
  <v-app dark>
    <v-app-bar
      fixed
      app
    >
      <v-toolbar-title v-text="title" />
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-bottom-navigation
      v-model="value"
      app
      grow
    >
      <v-btn to="/">
        <span>Home</span>
        <v-icon>mdi-home</v-icon>
      </v-btn>

      <v-btn @click="dialog.share = true">
        <span>Share</span>
        <v-icon>mdi-share-variant</v-icon>
      </v-btn>
    </v-bottom-navigation>
    <v-dialog
      v-model="dialog.share"
      width="500"
    >
      <v-card light>
        <v-card-title class="white--text primary mb-3">
          このアプリをシェアする
        </v-card-title>
        <v-card-text class="mt-2">
          <p>QRコードを読み込んでアプリをシェアしよう！</p>
          <v-img
            :src="require('~/assets/image/linkqr.png')"
            aspect-ratio="1.5"
            contain
          />
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      title: 'Porcepic Hub',
      value: 0,
      dialog: {
        share: false
      }
    }
  }
}
</script>

<style scoped>
#app .v-bottom-navigation .v-btn {
  height: inherit !important;
}
</style>
