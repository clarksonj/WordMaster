<template>
  <div id="app" v-on:keyup="checkAgainstSolution('abcd')">
    Current Row: {{currentRow}} <br />
    Current Letter: {{currentLetter}} <br />
    Win Condition: {{winCondition}}
    <GameRow
    v-for="(row, index) in guess"
    :key="index"
    :values="row "
    />
    <div v-if="failed" class="solution">{{solution}}</div>
  </div>
</template>

<script>
import { FourLetterWords } from '@/utilities/words.js'
import GameRow from './components/GameRow.vue'

export default {
  name: 'App',
  components: {
    GameRow
  },
  data() {
    return {
      stageSettings: {
        wordLength: 4,
        rowLength: 5,
      },
      currentRow: 0,
      currentLetter: 0,
      guess: [],
      winCondition: false,
      failed: false,
    };
  },
  created() {
    window.addEventListener('keyup', this.handleKeyUp);
    //eslint-disable-next-line
    this.guess = [...Array(this.stageSettings.rowLength)].map(x => Array(this.stageSettings.wordLength).fill(
      {value:''}
    ));
  },
  beforeDestroy() {
    window.removeEventListener('keyup', this.handleKeyUp);
  },
  computed: {
    solution() {
      // Generate Day solution
      return FourLetterWords[Math.floor(Math.random()*FourLetterWords.length)];
    },
    solutionSplit() {
      return [...this.solution] ;
    }
  },
  methods: {
    handleKeyUp(event) {
      if (this.winCondition) return;

      // Is A letter, Add to temp guess
      if (event.keyCode >= 65 && event.keyCode <= 90) {
        if (this.currentLetter === this.stageSettings.wordLength) return;
        const currentRow = [...this.guess[this.currentRow]]
        currentRow[this.currentLetter] = {value: event.key};
        this.$set(this.guess, this.currentRow, currentRow);
        this.currentLetter++;
      }

      // Delete Last added from temp guess
      if (event.keyCode === 8) {
        if (this.currentLetter === 0) return;
        const currentRow = [...this.guess[this.currentRow]]
        this.currentLetter--;
        currentRow[this.currentLetter] = {value: ''};
        this.$set(this.guess, this.currentRow, currentRow);
      }

      // Run Guess
      if (event.keyCode === 13 && this.currentLetter === this.stageSettings.wordLength) {
        this.checkAgainstSolution()
      }
    },
    checkAgainstSolution() {
      let currentRow = [...this.guess[this.currentRow]];
      const guess = currentRow.map(object => object.value).join('');

      if(!FourLetterWords.includes(guess)) {
        alert('not A word');
        return;
      }

      currentRow = currentRow.map((object, index) => {
        const value = object.value;

        if(value === this.solutionSplit[index]) {
          object.isCorrect = true;
        } else if(this.solutionSplit.includes(value)) {
          object.isClose = true;
        } else {
          object.isWrong = true;
        }
        return object
      })

      this.$set(this.guess, this.currentRow, currentRow);

      // Check Win Condition
      if (this.solution === guess) {
        this.winCondition = true;
        return;
      }

      if (this.currentRow + 1 === this.stageSettings.rowLength) {
        this.failed = true;
      }

      this.currentLetter = 0;
      this.currentRow++;
    },
  },
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  .solution {
    padding: 20px;
    font-size: 40px;
    text-transform: uppercase;
  }
}
</style>
