<script>
import { LETTER_DISTRIBUTION } from '../utils/alphabet.js'

const ASCII_A = 65;
const ASCII_Z = 90;

export default {
  data() {
    return {
      bag: [],
      board: [ "E", "N", "A", "P", "I", "S", "C", "I", "V" ],
      p_words: [],
      input_word: ""
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
    enterWord () {
      this.p_words.push(this.input_word)
      this.input_word = ''
    },
  },
  mounted() {
    this.initGame()
  }
}
</script>

<template>
  <v-container >

  <v-row>
    <v-col>
      <v-form @submit.prevent="enterWord">
        <v-text-field 
          v-model="input_word" 
          variant="underlined">
        </v-text-field>
      </v-form>
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
      <p>
        {{ board }}
      </p>
  </v-row>
  <v-row>
    <h1> Words: </h1>
    <p>
      {{ p_words }}
    </p>
  </v-row>
  </v-container>
  <!-- <div class="row">
      <h1> Bag has {{bag.length}} tiles remaining:</h1>
      <p> {{ bag }} </p>
  </div> -->
</template>

<style>
</style>
