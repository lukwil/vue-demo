<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card>
        <v-card-title class="headline">
          Welcome {{vorname}}!
        </v-card-title>
        <v-card-text>
          <v-text-field
          v-model="vorname"
            label="Vorname"
          ></v-text-field>
          <v-text-field
          v-model="nachname"
            label="Nachname"
          ></v-text-field>

          <name-output v-if="vollerName.trim()" :nameProp="vollerName"></name-output>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn color="error" disa @click="openAlert" :disabled="!nachname"> Open Alert </v-btn>
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
          ></v-text-field>
        <joke-output v-for="(joke, index) in jokes" :key="jokes[index]" :setup="joke.setup" :punchline="joke.punchline"></joke-output>
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

export default Vue.extend({
  components: {
   NameOutput,
   UpdateCounter,
  },
  data() {
    return {
      vorname: '',
      nachname: '',
      search: '',
      jokes: [{setup: '', punchline: ''}],
      awaitSearch: false,
      loading: false,
    }
  }, 
  computed: {
    vollerName(): string {
      return `${this.vorname} ${this.nachname}`
    }
  },
  watch: {
    search(searchString: string) {
      if (!this.awaitSearch) {
        setTimeout(() => {
          console.log(`search string: ${searchString}`)
          this.getDataFromApi(searchString)
          this.awaitSearch = false
        }, 1200) // 1.2 sec delay
      }
      this.awaitSearch = true
    },
  },
  methods: {
    openAlert() {
      this.getDataFromApi('programming')
      //alert('Test-Alert')
    },
    getDataFromApi(searchString: string) {
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
