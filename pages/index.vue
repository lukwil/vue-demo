<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card>
        <v-card-title class="headline">
          Welcome {{firstName}}!
        </v-card-title>
        <v-card-text>
          <v-text-field
          v-model="firstName"
            label="Vorname"
          ></v-text-field>
          <v-text-field
          v-model="lastName"
            label="Nachname"
          ></v-text-field>
          <update-counter></update-counter>
          <name-output v-if="fullName.trim()" :nameProp="fullName"></name-output>
          
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn color="error" @click="openAlert" :disabled="!lastName"> Open Alert </v-btn>
          <v-btn color="primary" nuxt to="/inspire"> Continue </v-btn>
        </v-card-actions>
      </v-card>

    </v-col>
    <v-col cols="12" sm="8" md="6">
      <v-card>
        <v-card-title class="headline">
          Joke
        </v-card-title>
        <v-card-text>
          <v-text-field
          v-model="search"
            label="Search"
            :loading="loading"
            @input="isTyping = true"
          ></v-text-field>
        <joke-output v-for="(joke, index) in jokes" :key="index" :setup="joke.setup" :punchline="joke.punchline"></joke-output>
          <!-- <name-output v-if="vollerName.trim()" :nameProp="vollerName"></name-output> -->
        </v-card-text>
      </v-card>
      
    </v-col>
  </v-row>
</template>

<script lang="ts">
import Vue from 'vue'
import NameOutput from '~/components/NameOutput.vue'
import JokeOutput from '~/components/JokeOutput.vue'
import UpdateCounter from '~/components/UpdateCounter.vue'
import {debounce} from 'debounce'

export default Vue.extend({
  components: {
   NameOutput,
   UpdateCounter,
  },
  data() {
    return {
      firstName: '',
      lastName: '',
      search: '',
      isTyping: false,
      jokes: [],
      awaitSearch: false,
      loading: false,
    }
  }, 
  computed: {
    fullName(): string {
      return `${this.firstName} ${this.lastName}`
    }
  },
  watch: {
    search: debounce(function() {
      this.isTyping = false;
    }, 1000),
    isTyping(value: boolean) {
      console.log(value)
      if (!value) {
        this.getDataFromApi(this.search);
      }
    }
  },
  methods: {
    openAlert() {
      alert('Test-Alert')
    },
    getDataFromApi(searchString: string) {
      console.log(`search string: ${searchString}`)
      this.loading = true;
      // https://official-joke-api.appspot.com/jokes/programming/ten
      const path = `https://official-joke-api.appspot.com/jokes/${searchString}/ten`;
      this.$axios.$get(path).then((res: any) => {
        console.log(res)
        this.jokes = res;
        this.loading = false;
      });
    }
  },
})
</script>
