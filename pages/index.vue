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
          <v-col cols="6">
            <v-card to="/tequila">
              <v-img
                :src="require('~/assets/image/tequila.png')"
                alt="Tequila Legend"
                class="ma-1"
              />
              <v-card-title>Tequila Legend</v-card-title>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
}
</script>
