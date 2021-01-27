<template>
  <v-card :to="to">
    <v-img
      :src="src"
      :alt="title"
      class="ma-1"
    />
    <v-card-title>{{ title }}</v-card-title>
  </v-card>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: 'unknown game'
    },
    src: {
      type: String,
      default: ''
    },
    to: {
      type: String,
      default: '/'
    }
  }
}
</script>
