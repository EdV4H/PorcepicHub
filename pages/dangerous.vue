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
            :settings="params"
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
          color="orange"
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
    <template v-if="params.scene.value === scenes.game">
      <v-btn icon @click="backToTitle()">
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>
      <v-card
        class="mt-2"
        color="grey darken-3"
        max-width="600"
      >
        <v-card-title>
          <v-icon
            color="red lighten-2"
            class="mr-12"
            size="64"
          >
            mdi-bomb
          </v-icon>
          <v-row align="start">
            <div class="caption grey--text text-uppercase">
              Explosion Rate
            </div>
            <div>
              <span
                class="display-2 font-weight-black"
                v-text="exrate || '—'"
              />
              <strong v-if="exrate">%</strong>
            </div>
          </v-row>
        </v-card-title>
      </v-card>
      <v-card
        color="grey darken-3"
        class="mt-5"
      >
        <v-row class="mx-0">
          <v-col
            v-for="i in params.map.value.length"
            :key="i"
            :cols="12 / params.edge.value"
            class="text-center"
          >
            <base-once-btn
              fab
              color="red"
              @click="tap(i-1)"
            />
          </v-col>
        </v-row>
      </v-card>
    </template>
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
      src: require('~/assets/image/dangerous.png'),
      params: {
        scene: {
          type: 'invisible',
          value: 0
        },
        edge: {
          type: 'slider',
          label: '一辺のボタン数',
          value: 4,
          max: 4,
          min: 1
        },
        map: {
          type: 'invisible',
          value: []
        },
        safecnt: {
          type: 'invisible',
          value: 0
        }
      }
    }
  },
  computed: {
    exrate () {
      return Math.round(1 / (this.params.map.value.filter(i => i === -1).length + 1) * 10000) / 100
    }
  },
  mounted () {
    this.audio.bgm = new Audio('https://raw.githubusercontent.com/EdV4H/PorcepicHub/main/assets/sound/bgm/dangerous.mp3')
    this.audio.bgm.loop = true
    this.audio.tap = new Audio('https://raw.githubusercontent.com/EdV4H/PorcepicHub/main/assets/sound/se/tap.mp3')
    this.audio.gameover = new Audio('https://raw.githubusercontent.com/EdV4H/PorcepicHub/main/assets/sound/se/bomb.mp3')
  },
  methods: {
    play () {
      this.initGame()
    },
    backToTitle () {
      this.dialog.gameover = false
      this.audio.bgm.pause()
      this.audio.bgm.currentTime = 0
      this.params.scene.value = this.scenes.title
    },
    initGame () {
      this.params.scene.value = this.scenes.game
      this.dialog.gameover = false

      this.params.map.value = Array.from({ length: this.params.edge.value * this.params.edge.value }, () => -1)
      this.params.map.value[Math.floor(Math.random() * this.params.map.value.length)] = 1

      this.audio.bgm.currentTime = 0
      // this.audio.bgm.play()
    },
    tap (i) {
      this.audio.tap.currentTime = 0
      this.audio.tap.play()
      if (this.params.map.value[i] === 1) {
        // game over
        this.params.scene.value = this.scenes.gameover
        this.dialog.gameover = true
        this.audio.bgm.pause()
        this.audio.gameover.play()
      }
      // vue's computed can not detected this
      // this.params.map.value[x * this.params.edge.value + y] = 0
      this.params.map.value.splice(i, 1, 0)
    }
  }
}
</script>
