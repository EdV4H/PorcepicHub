<template>
  <v-app dark>
    <v-bottom-navigation
      v-model="value"
      :background-color="color"
      app
      grow
      shift
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
          ></v-img>
        </v-card-text>
      </v-card>
    </v-dialog>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      value: 0,
      dialog: {
        share: false
      }
    }
  },
  computed: {
    color () {
      switch (this.value) {
        case 0: return 'blue-grey'
        case 1: return 'teal'
        case 2: return 'brown'
        case 3: return 'indigo'
        default: return 'blue-grey'
      }
    }
  }
}
</script>
