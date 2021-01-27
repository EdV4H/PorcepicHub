<template>
  <v-card>
    <v-card-title class="mb-3">
      このアプリをシェアする
      <v-spacer />
      <v-btn
        icon
        @click="copyURL()"
      >
        <v-icon>
          mdi-content-copy
        </v-icon>
      </v-btn>
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
</template>

<script>
export default {
  methods: {
    copyURL () {
      navigator.clipboard.writeText('https://www.porcepichub.com/')
    }
  }
}
</script>