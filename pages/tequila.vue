<template>
  <div>
    <v-row v-if="params.scene === scenes.title">
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
          :src="require('~/assets/image/tequila.png')"
          alt="Tequila Legend"
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
    <v-row v-if="params.scene === scenes.game">
      <v-col>
        <v-btn icon @click="backToTitle()">
          <v-icon>mdi-arrow-left</v-icon>
        </v-btn>
      </v-col>
      <v-col cols="12">
        <v-card
          color="grey darken-3"
          max-width="600"
        >
          <v-card-title>
            <v-icon
              color="red lighten-2"
              class="mr-12"
              size="64"
            >
              mdi-gesture-double-tap
            </v-icon>
            <v-row align="start">
              <div class="caption grey--text text-uppercase">
                Tap rate
              </div>
              <div>
                <span
                  class="display-2 font-weight-black"
                  v-text="sum*6 || '—'"
                />
                <strong v-if="sum">TPM</strong>
              </div>
            </v-row>
          </v-card-title>

          <v-sheet color="transparent">
            <v-sparkline
              :key="String(avg)"
              :smooth="8"
              :gradient="['#f72047', '#ffd200', '#1feaea']"
              :line-width="3"
              :value="params.tpm"
              auto-draw
              fill
              :auto-draw-duration="0"
              stroke-linecap="round"
            />
          </v-sheet>
        </v-card>
      </v-col>
      <v-col cols="12">
        <v-card
          color="grey darken-3"
          max-width="600"
        >
          <v-card-title>
            <v-icon
              color="red lighten-2"
              class="mr-12"
              size="64"
            >
              mdi-flash
            </v-icon>
            <v-row align="start">
              <div class="caption grey--text text-uppercase">
                Max Tap
              </div>
              <div>
                <span
                  class="display-2 font-weight-black"
                  v-text="params.maxcnt || '—'"
                />
                <strong v-if="params.maxcnt">TPT</strong>
              </div>
            </v-row>
          </v-card-title>
        </v-card>
      </v-col>
      <v-col cols="8">
        <v-btn block height="150px" color="pink" dark @click="tap()">
          <v-icon x-large left>
            mdi-gesture-tap
          </v-icon>
          <p class="text-h4 mt-4 font-weight-bold">
            Tap!
          </p>
        </v-btn>
      </v-col>
      <v-col cols="4">
        <v-btn
          block
          height="150px"
          color="primary"
          dark
          :disabled="params.hell ? params.turncnt <= params.maxcnt : params.turncnt <= 0"
          @click="nextTurn()"
        >
          <v-icon size="50">
            mdi-arrow-right
          </v-icon>
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
      gameTitle: 'Tequila Legends',
      settings: [],
      params: {
        scene: 0,
        tapcnt: 0,
        temp: 0,
        turncnt: 0,
        maxcnt: 0,
        trigger: 0,
        maxtrigger: 30,
        tpm: [],
        hell: false
      },
      taplogid: undefined
    }
  },
  computed: {
    avg () {
      const sum = this.params.tpm.reduce((acc, cur) => acc + cur, 0)
      const length = this.params.tpm.length

      if (!sum && !length) { return 0 }

      return Math.ceil(sum / length)
    },
    sum () {
      return this.params.tpm.reduce((acc, cur) => acc + cur, 0)
    }
  },
  mounted () {
    this.genSettings()
    this.audio.bgm = new Audio('https://raw.githubusercontent.com/EdV4H/PorcepicHub/main/assets/sound/bgm/tequila.mp3')
    this.audio.bgm.loop = true
    this.audio.tap = new Audio('https://raw.githubusercontent.com/EdV4H/PorcepicHub/main/assets/sound/se/tap.mp3')
    this.audio.gameover = new Audio('https://raw.githubusercontent.com/EdV4H/PorcepicHub/main/assets/sound/se/tequila.mp3')
  },
  methods: {
    play () {
      this.applySettings()
      this.initGame()
      this.params.scene = this.scenes.game
    },
    backToTitle () {
      this.dialog.gameover = false
      clearInterval(this.taplogid)
      this.audio.bgm.pause()
      this.audio.bgm.currentTime = 0
      this.params.scene = this.scenes.title
    },
    initGame () {
      this.dialog.gameover = false
      this.params.tapcnt = 0
      this.params.temp = 0
      this.params.turncnt = 0
      this.params.maxcnt = 0
      this.params.trigger = Math.ceil(Math.random() * this.params.maxtrigger)
      this.params.tpm = new Array(10).fill(0)
      this.taplogid = setInterval(this.taplog, 1000)
      this.audio.bgm.currentTime = 0
      this.audio.bgm.play()
    },
    tap () {
      this.audio.tap.currentTime = 0
      this.audio.tap.play()
      this.params.tapcnt++
      this.params.temp++
      this.params.turncnt++
      if (this.params.tapcnt > this.params.trigger) {
        this.dialog.gameover = true
        clearInterval(this.taplogid)
        this.audio.bgm.pause()
        this.audio.gameover.play()
      }
    },
    nextTurn () {
      if (this.params.turncnt > this.params.maxcnt) {
        this.params.maxcnt = this.params.turncnt
      }
      this.params.turncnt = 0
    },
    taplog () {
      this.params.tpm.shift()
      this.params.tpm.push(this.params.temp)
      this.params.temp = 0
    },
    genSettings () {
      this.settings.push({
        label: '最大タップ回数',
        value: this.params.maxtrigger,
        type: 'slider',
        max: 300,
        min: 1
      })
      this.settings.push({
        label: 'HELL mode',
        value: this.params.hell,
        type: 'switch'
      })
    },
    applySettings () {
      this.params.maxtrigger = this.settings[0].value
      this.params.hell = this.settings[1].value
    }
  }
}
</script>
