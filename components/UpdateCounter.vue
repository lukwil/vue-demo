<template>
  <v-chip
      class="ma-2"
      color="secondary"
    >
      Updates in NameOutput: {{counter}}
    </v-chip>
</template>
<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  computed: {
    counter() {
      return this.$store.state.counter
    }
  }
})
</script>