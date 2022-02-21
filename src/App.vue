<template>
  <div id="app" class="flex justify-center m-5">
    <div class="w-10/12">
      <div role="alert" v-if="error" class="m-5">
        <div class="bg-red-500 text-white font-bold rounded-t px-4 py-2">
          Error
        </div>
        <div v-html="error" class="border border-t-0 border-red-400 rounded-b bg-red-100 px-4 py-3 text-red-700">
        error text
        </div>
      </div>
      <div v-if="isGameOver"  class="flex items-center justify-center bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative" role="alert">
        YOU DIED <span class="text-4xl ml-3">&#128546;</span>
      </div>
      <div v-if="isWin"  class="flex items-center justify-center bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded relative" role="alert">
        YOU WIN <span class="text-4xl ml-3">&#128512;</span>
      </div>
      <div class="container w-fit m-auto border border-slate-300 bg-slate-100 p-5">
        <div>lives: {{lives}}</div>
        <div>tried: {{lettersTried.join(' ')}}</div>
        <Word class="flex justify-center" :word="word" :isWin="isWin" :isGameOver="isGameOver"/>
        <form v-on:submit.prevent="" class="flex justify-center">
          <input :disabled="isGameOver" class="border border-slate-200 m-5"
                 v-model="guess" placeholder="enter a guess"
                 maxlength="1">
          <button :disabled="isGameOver" type="submit" @click="submitForm">Submit</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import Word from './components/Word.vue'
var randomWords = require('random-words');

export default {
  name: 'App',
  components: {
    Word
  },
  data() {
    return {
        error: '',
        guess: '',
        lives: 10,
        lettersTried: [],
        word: [],
        template_word: {
          id: 0,
          found: 0,
          value: ''
        }
      }
  },
  methods:{
    setWord: function(newWord) {
      for (let i = 0; i < newWord.length; i++) {
        let letter = (newWord.charAt(i));
        this.word.push(
          {
            id: i,
            found: 0,
            value: letter
          }
        );
      }
    },
    findLettersInWord: function(letter){
      let found = false;
      for (let i = 0; i < this.word.length; i++) {
        if(this.word[i].value === letter){
          this.word[i].found = 1;
          found = true;
        }
      }
      return found;
    },
    loseALife: function() {
      this.lives -= 1;
    },
    resetGuess: function(){
      this.guess = '';
    },
    submitForm: function() {
      let found = false;
      for (let i = 0; i < this.guess.length; i++) {
        let letter = this.guess[i];
        if(this.findLettersInWord(letter)){
          found = true;
        }else{
          if (!this.lettersTried.includes(letter)) {
            this.lettersTried.push(letter);
          }
        }
      }
      if (found !== true) {
        this.loseALife();
        this.resetGuess();
      }else{
        this.resetGuess();
      }
    },
  },
  computed: {
    isWin() {
      if (this.word.length === 0){
        return false;
      }
      for (let i = 0; i < this.word.length; i++) {
        if(this.word[i].found === 0){
          return false;
        }
      }
      return true;
    },
    isGameOver() {
      return this.lives === 0;
    },
  },
  created: function(){
    this.setWord(randomWords());
    // this.setWord('test');
  },
}
</script>