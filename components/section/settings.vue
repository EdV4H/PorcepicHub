<template>
  <v-card>
    <v-card-title class="primary mb-3">
      {{ gameTitle }} 設定
    </v-card-title>

    <v-card-text>
      <v-row
        v-for="(setting, i) in settings"
        :key="i"
      >
        <v-col v-if="setting.type === 'slider'">
          <v-subheader class="pl-0">
            {{ setting.label }}
          </v-subheader>
          <v-slider
            v-model="setting.value"
            :min="setting.min"
            :max="setting.max"
            thumb-label
          />
        </v-col>
        <v-col v-if="setting.type === 'switch'">
          <v-switch
            v-model="setting.value"
            :label="setting.label"
            color="red darken-3"
            hide-details
          />
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    gameTitle: {
      type: String,
      default: 'No Title'
    },
    settings: {
      type: Object,
      default: undefined
    }
  }
}
</script>
