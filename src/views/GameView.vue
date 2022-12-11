<script>
import { LETTER_DISTRIBUTION } from '../utils/letterDistribution.js'
import { isValidWord } from '../utils/validateWord.js'

const ASCII_A = 65;
const ASCII_Z = 90;

export default {
  data() {
    return {
      bag: [],
      board: [ ],
      // board: [ "B", "R", "A", "L", "E", "O", "X", "S", "C", "L", "O", "T", "H", "E"],
      p_words: [],
      p_words: ["BRAND", "HEAR"],
      input_word: '',
      selected_word: ''
    }
  },
  methods: {
    initGame() {
      for (let i = ASCII_A; i <= ASCII_Z; i++) {
        var letter = String.fromCharCode(i)
        for (let j = 0; j <= LETTER_DISTRIBUTION[letter]; j++) {
          this.bag.push(letter)
        }
      }
    },
    flipTile() {
      const random = Math.floor(Math.random() * this.bag.length);
      const tile = this.bag.splice(random, 1)[0];
      this.board.push(tile)
    },
    enterWord() {
      this.input_word = this.input_word.toUpperCase();
      if (isValidWord(this.input_word, this.board, this.selected_word, this.p_words)){
        this.p_words.push(this.input_word)
      }
      this.input_word = ''
      this.selected_word = ''
    },
    selectExistingWord(word) {
      this.selected_word = word
    }
  },
  mounted() {
    this.initGame()

    window.addEventListener('keydown', e => {
      if (e.keyCode >= 65 && e.keyCode <= 90) { // letter pressed
        this.input_word += String.fromCharCode(e.keyCode);
      }
      else if (e.keyCode == 38) { // up arrow
        this.flipTile();
      }
      else if (e.keyCode == 13) { // enter
        this.enterWord();
      }
      else if (e.keyCode == 8 || e.keyCode == 46) { // delete key
        this.input_word = this.input_word.slice(0, -1); 
      }
    });
  }
}

</script>

<template>
  <v-container >
  <v-row>
    <v-col>
      <p>{{input_word}}</p>
      <!-- <v-form @submit.prevent="enterWord">
        <v-text-field 
          v-model="input_word" 
          variant="underlined">
        </v-text-field>
      </v-form> -->
    </v-col>
    <v-col lg="2">
      <v-btn @click="enterWord" width="170px">Enter Word</v-btn>
    </v-col>
    <v-col>
      <v-btn @click="flipTile" width="170px">Flip Tile</v-btn>
    </v-col>
  </v-row>
  <v-row>
      <h1> Board: </h1>
      <v-container>
        <v-row>
          <v-col cols="12" sm="1" md="1" v-for="letter in board" >
            <div class="tile"><p>{{letter}}</p></div>
          </v-col>
        </v-row>
      </v-container>
  </v-row>
  <v-row>
    <h1> Words: </h1>
    <v-container>
      <v-row>
        <v-col cols="12" md="3" v-for="word in p_words" >
          
          <v-btn x-large @click="selectExistingWord(word)" ><p>{{word}}</p> </v-btn>
        </v-col>
      </v-row>
    </v-container>

  </v-row>
  </v-container>
  <!-- <div class="row">
      <h1> Bag has {{bag.length}} tiles remaining:</h1>
      <p> {{ bag }} </p>
  </div> -->
</template>

<style>
.tile {
  background-color: black;
  color: var(--fg-primary);
  text-align: center;
  width: 80%;
}
.tile p{
  color: white;
  font-weight: bold;
}

</style>
