<template>
  <v-row>
    <v-col class="text-center">
      <v-img
        :src="require('~/assets/image/tequila.png')"
        alt="Vuetify.js"
        class="mb-5"
      >
      </v-img>
    </v-col>
    <v-col>
      <v-btn @click="play">
        play
      </v-btn>
    </v-col>
  </v-row>
</template>

<script>
export default {
  layout: 'game',
  data () {
    return {
      bgm: undefined
    }
  },
  mounted () {
    this.bgm = new Audio('https://raw.githubusercontent.com/EdV4H/PorcepicHub/main/assets/sound/bgm/tequila.mp3')
  },
  methods: {
    play () {
      this.bgm.play()
    }
  }
}
</script>
