<template>
  <v-app dark>
    <v-app-bar
      fixed
      app
    >
      <v-img
        class="mt-2"
        :src="require('~/assets/image/banner.png')"
        max-height="50"
        contain
      />
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <base-bottom-navigation />
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      title: 'Porcepic Hub'
    }
  }
}
</script>
