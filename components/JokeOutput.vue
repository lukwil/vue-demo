<template>
<div>
  <v-card elevation="26" color="#385F73"
            dark>
    <v-card-title>{{setup}}</v-card-title>
    <v-card-text>{{punchline}}</v-card-text>
  </v-card>
  <v-divider></v-divider>
</div>
</template>
<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  props: {
    setup: {
      type: String,
      required: true,
    },
    punchline: {
      type: String,
      required: true,
    }
  }
})
</script>