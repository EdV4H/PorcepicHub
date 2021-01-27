<template>
  <div>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="8" md="6">
        <v-card class="mb-3">
          <v-card-title class="headline">
            Welcome to the Porcepic Hub
          </v-card-title>
          <v-card-text>
            <p>Porcepic Hubは飲み会をちょっと楽しくするために作られたWebアプリケーションです．</p>
            <p>
              Find a bug? Report it on the github <a
                href="https://github.com/EdV4H/PorcepicHub/issues"
                target="_blank"
                rel="noopener noreferrer"
                title="contribute"
              >
                issue board
              </a>.
            </p>
            <div class="text-xs-right">
              <em><small>&mdash; Yusuke Maruyama</small></em>
            </div>
          </v-card-text>
        </v-card>
        <v-row align="center">
          <v-col
            v-for="(game, i) in games"
            :key="i"
            cols="6"
          >
            <base-content-card
              :title="game.title"
              :src="game.src"
              :to="game.to"
            />
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  data () {
    return {
      games: [
        {
          title: 'Tequila Legend',
          src: require('~/assets/image/tequila.png'),
          to: '/tequila'
        },
        {
          title: 'Dangerous One',
          src: require('~/assets/image/dangerous.png'),
          to: '/dangerous'
        }
      ]
    }
  }
}
</script>
