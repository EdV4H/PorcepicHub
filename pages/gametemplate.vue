<template>
  <div>
    <v-row v-if="params.scene.value === scenes.title">
      <v-col>
        <v-dialog
          v-model="dialog.settings"
          width="500"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              icon
              large
              v-bind="attrs"
              v-on="on"
            >
              <v-icon large>
                mdi-cog
              </v-icon>
            </v-btn>
          </template>
          <section-settings
            :game-title="gameTitle"
            :settings="settings"
          />
        </v-dialog>
      </v-col>
      <v-col class="text-center">
        <v-img
          :src="src"
          :alt="gameTitle"
          class="my-5"
        />
      </v-col>
      <v-col>
        <v-btn
          block
          x-large
          color="pink"
          @click="play"
        >
          <v-icon left size="25">
            mdi-play
          </v-icon>
          <span class="text-h6">
            play
          </span>
        </v-btn>
      </v-col>
    </v-row>
    <v-row v-if="params.scene.value === scenes.game">
      <v-col>
        <v-btn icon @click="backToTitle()">
          <v-icon>mdi-arrow-left</v-icon>
        </v-btn>
      </v-col>
    </v-row>
    <v-dialog
      v-model="dialog.gameover"
      width="500"
      persistent
    >
      <v-card light color="lighten-1">
        <v-card-text>
          <v-row>
            <v-col cols="12">
              <v-img :src="require('~/assets/image/tequilagameover.png')" contain height="180" />
            </v-col>
            <v-col cols="6" class="text-center">
              <v-btn fab large color="primary" @click="backToTitle()">
                <v-icon>mdi-home</v-icon>
              </v-btn>
            </v-col>
            <v-col cols="6" class="text-center">
              <v-btn fab large color="success" @click="initGame()">
                <v-icon>mdi-replay</v-icon>
              </v-btn>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  layout: 'game',
  data () {
    return {
      scenes: {
        title: 0,
        game: 1,
        gameover: 2
      },
      dialog: {
        settings: false,
        gameover: false
      },
      audio: {
        bgm: undefined,
        tap: undefined,
        gameover: undefined
      },
      gameTitle: 'Game Template',
      src: require('~/assets/image/tequila.png'),
      params: {
        scene: {
          type: 'invisible',
          value: 0
        }
      }
    }
  },
  computed: {
  },
  mounted () {
    this.audio.bgm = new Audio('https://raw.githubusercontent.com/EdV4H/PorcepicHub/main/assets/sound/bgm/tequila.mp3')
    this.audio.bgm.loop = true
    this.audio.tap = new Audio('https://raw.githubusercontent.com/EdV4H/PorcepicHub/main/assets/sound/se/tap.mp3')
    this.audio.gameover = new Audio('https://raw.githubusercontent.com/EdV4H/PorcepicHub/main/assets/sound/se/tequila.mp3')
  },
  methods: {
    play () {
      this.initGame()
      this.params.scene.value = this.scenes.game
    },
    backToTitle () {
      this.dialog.gameover = false
      this.audio.bgm.pause()
      this.audio.bgm.currentTime = 0
      this.params.scene.value = this.scenes.title
    },
    initGame () {
      this.dialog.gameover = false
      this.audio.bgm.currentTime = 0
      this.audio.bgm.play()
    }
  }
}
</script>
