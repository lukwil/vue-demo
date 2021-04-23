<template>
  <v-chip
      class="ma-2"
      color="primary"
    >
      {{nameProp}}
    </v-chip>
</template>
<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  props: {
    nameProp: {
      type: String,
      required: true,
    }
  }
})
</script>
