<template>
  <v-btn
    :disabled="disabled"
    v-bind="{
      rows: 3,
      ...$attrs,
    }"
    v-on="$listeners"
    @click="disabled = true"
  >
    <slot />
  </v-btn>
</template>

<script>
export default {
  data () {
    return {
      disabled: false
    }
  }
}
</script>
