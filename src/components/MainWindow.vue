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
            <v-btn @click="searchInWiki" text color="deep-purple accent-4">Wiki</v-btn>
          </v-card-actions>            
        </v-card>
        <p class="text-center font-weight-light email">send nudes: mestervalera@gmail.com</p>
        
        <v-card
          v-if="showWiki"
          max-width="700"
          class="mx-auto"
        >
          <v-card-text>
            <div class="wiki"></div>
          </v-card-text>

        </v-card>
        
        
      </v-flex>
    </v-layout>

    
  </v-app>
  
</template>

<script>

import words from '../wordbase/words.js';
import axios from "axios";

export default {
  data: () => ({
    words,
    symbols: [],
    correct: '',
    select : '',
    type : 'success',
    isUserSelect: false,
    success: 0,
    errors: 0,
    showWiki: false
  }),
  methods: {
    userSelect(i){
      return i
    },

    createCard() {
      this.showWiki = false
      this.isUserSelect = false
      const n = Math.floor(Math.random() * (this.words.length ));
      const randomItem = this.words[n];
      const word = randomItem.word.split("");
      this.symbols = word;
      this.correct = randomItem.correct
    },

    checkStress(symb){
      if (symb == this.correct){
        this.type = "success"
        this.isUserSelect = true
        this.success+=1
        setTimeout(this.createCard, 500)
      } else {
        this.type = "error"
        this.isUserSelect = true
        this.errors+=1
      }
    },
    searchInWiki: function() {
      this.showWiki = true
      axios
        .post("https://word-stress.herokuapp.com/wiki", {
          word: this.symbols.join("")
        })
        .then(response => {
          const resHtml = response.data
          console.log(resHtml)
          document.querySelector('.wiki').innerHTML = resHtml          
        })
      
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
.email{
  margin-top: 10px;
}
</style>
