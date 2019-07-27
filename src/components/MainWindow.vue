<template>
  <v-app id="word-stress">
    <v-layout align-center justify-center>
      <v-flex xs12 >
        <v-card class="mx-auto" max-width="500">
          <v-toolbar color="cyan" dark >
              <v-app-bar-nav-icon></v-app-bar-nav-icon>
              <v-toolbar-title>Ударения</v-toolbar-title>
            </v-toolbar>
          <v-card-text>
            <v-alert
              v-if="isUserSelect == false"
              color="indigo lighten-1"
              dark
            >
              Укажи правильное ударение
            </v-alert>
            <v-alert
              v-if="isUserSelect"
              :type="type"
              class="mb-4"
            >
            {{type}}
          </v-alert>
            <div>Слово</div>
            <p class="word display-1 text--primary">
              <span v-for="(symb, i) in symbols" 
              :key="i" 
              @click="checkStress(i)" 
              class="word-symbol">
                {{symb}}
              </span>
            </p>

            <p>Счет</p>
            <v-chip
              class="counter"
              color="green"
              text-color="white"
            >
              {{success}}
            </v-chip>

            <v-chip
              class="counter"
              color="red"
              text-color="white"
            >
              {{errors}}
            </v-chip>
            
          </v-card-text>
          <v-card-actions>
            <v-btn @click="createCard" text color="deep-purple accent-4">Next word</v-btn>
          </v-card-actions>            
        </v-card>
        <p class="text-center font-weight-light">mestervalera@gmail.com</p>
      </v-flex>
    </v-layout>
  </v-app>
  
</template>

<script>
import words from '../wordbase/words.js';
export default {
  data: () => ({
    words,
    symbols: [],
    correct: '',
    select : '',
    type : 'success',
    isUserSelect: false,
    success: 0,
    errors: 0
  }),
  methods: {
    userSelect(i){
      return i
    },

    createCard() {
      this.isUserSelect = false
      const n = Math.floor(Math.random() * (this.words.length ));
      const randomItem = this.words[n];
      const word = randomItem.word.split("");
      this.symbols = word;
      this.correct = randomItem.correct
    },

    checkStress(symb){
      console.log(symb, this.correct)
      if (symb == this.correct){
        console.log("cool")
        this.type = "success"
        this.isUserSelect = true
        this.success+=1
        setTimeout(this.createCard, 500)
      } else {
        console.log("not cool")
        this.type = "error"
        this.isUserSelect = true
        this.errors+=1
      }
    }
  },
  mounted(){
    this.createCard()
  }
};
</script>

<style scoped>
.word-symbol{
  color: rgb(100, 99, 99);
}

.word-symbol:hover{
  color: black;
  cursor: pointer;
}
.counter{
  margin-right: 5px;
  padding: 0 30px;
}
</style>
