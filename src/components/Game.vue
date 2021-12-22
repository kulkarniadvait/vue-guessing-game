<template>
  <v-container>
    <v-card max-width="50%" class="mx-auto pa-5">
      <h2>Guessing game</h2>
      <br />
      <div>
        <p>I am thinking a number from 1 to 10</p>
        <p>You must guess what it is in three tries</p>
        <v-form v-model="valid" ref="form">
          <v-text-field
            label="Guess"
            placeholder="Enter your guess"
            v-model="userInput"
            :rules="guessRules"
          >
            <template v-slot:append-outer
              ><v-btn
                @click="checkGuess"
                color="success"
                :disabled="disableGuess || !valid"
                >Guess</v-btn
              ></template
            >
          </v-text-field>
        </v-form>
        <div v-for="(input, index) in inputs" :key="index">
          <div v-if="input.correct">
            <p>Guess {{ index + 1 }} : {{ input.value }}</p>
          </div>
          <div v-else>
            <p>
              Guess {{ index + 1 }} : {{ input.value }} ({{ input.proximity }})
            </p>
          </div>
        </div>
        <div v-if="correct">
          <p>You guessed the right answer!</p>
        </div>
        <div v-else-if="inputs.length >= 3">
          <p>You lost the game! The correct answer was {{ secret }}</p>
        </div>
        <v-btn @click="resetGame" color="info">Reset Game</v-btn>
      </div>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: "Game",
  data() {
    return {
      valid: false,
      inputs: [],
      guessRules: [
        (userInput) =>
          (userInput <= 10 && userInput >= 1) ||
          "You must guess between 1 to 10",
      ],
      secret: null,
      userInput: null,
      guess: null,
      correct: false,
      proximity: null,
      disableGuess: false,
    };
  },

  mounted() {
    this.generateSecretNumber();
  },

  methods: {
    generateSecretNumber() {
      this.secret = Math.floor(Math.random() * 10) + 1;
      console.log(this.secret);
    },

    checkGuess() {
      this.guess = this.userInput;
      this.userInput = "";
      if (this.guess == this.secret) {
        this.correct = true;
        this.disableGuess = true;
      } else if (
        this.guess == this.secret - 1 ||
        this.guess == this.secret + 1
      ) {
        this.proximity = "hot";
      } else if (this.guess < this.secret - 3 || this.guess > this.secret + 3) {
        this.proximity = "cold";
      } else {
        this.proximity = "warm";
      }
      this.inputs.push({
        value: this.guess,
        proximity: this.proximity,
        correct: this.correct,
      });
      if (this.inputs.length == 3) {
        this.disableGuess = true;
      }
      this.$refs.form.reset();
    },

    resetGame() {
      this.inputs = [];
      this.generateSecretNumber();
      this.correct = false;
      this.disableGuess = false;
      this.$refs.form.reset();
    },
  },
};
</script>
