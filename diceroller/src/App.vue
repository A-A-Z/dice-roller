<template>
  <main id="app">
    <header>
      <h1>Dice Roller</h1>
      <hr>
    </header>
    <section class="dice-form">
      <input type="number" min="1" max="6" v-model="number" @change="buildDice">
      <button v-on:click="roll">Roll</button>
    </section>
    <section class="dice-results">
      <ul class="results-list">
        <li v-for="(die, index) in dice">
          <Die :result="die" :rolling="rolling[index]" />
        </li>
      </ul>
    </section>
    <section class="dice-totals">
      {{ sumTxt }} = <strong>{{ total }}</strong>
    </section>
  </main>
</template>

<script>
import Die from './components/Die'

let data = {
  number: 3,
  dice: [],
  rolling: [],
  sumTxt: '',
  total: 0
}
let timers = []
let rollsToGo = []

export default {
  name: 'app',
  components: { Die },
  data: () => {
    return data
  },
  methods: {
    buildDice: () => {
      let dice = []
      let rolling = []

      for (let i = 0; i < data.number; i++) {
        dice.push(1)
        rolling.push(false)
      }

      data.dice = dice
      data.sumTxt = dice.join(' + ')
      data.total = dice.length
    },
    roll: (event) => {
      data.total = 0

      const keepRolling = (index) => {
        let dice = data.dice.slice()
        let rolling = data.rolling.slice()
        const newNumber = Math.floor((Math.random() * 6) + 1)
        dice[index] = newNumber
        data.dice = dice
        data.sumTxt = dice.join(' + ')
        data.total = dice.reduce((a, b) => a + b, 0)

        rollsToGo[index]--

        if (rollsToGo[index] === 0) {
          rolling[index] = false
          clearInterval(timers[index])
        } else if (rollsToGo[index] < 2) {
          rolling[index] = false
        } else {
          rolling[index] = true
        }

        data.rolling = rolling
      }

      if (data.rolling.reduce((a, b) => a + b, 0) === 0) {
        for (let i = 0; i < data.number; i++) {
          rollsToGo[i] = Math.floor((Math.random() * 10) + 7)
          timers[i] = setInterval(() => { keepRolling(i) }, 70)
        }
      }
    }

  },
  mounted () {
    this.buildDice()
  }
}
</script>

<style lang="scss">
   @import './assets/sass/app.scss'
</style>
