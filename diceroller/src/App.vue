<template>
  <main id="app">
    <header>
      <h1>Dice Roller</h1>
    </header>
    <section>
      <input type="number" min="1" max="6" v-model="number" @change="buildDice">
      <button v-on:click="roll">Roll</button>
    </section>
    <section>
      <ul>
        <li v-for="die in dice">
          {{ die }}
        </li>
      </ul>
    </section>
    <section>
      {{ sumTxt }} = {{ total }}
    </section>
  </main>
</template>

<script>

let data = {
  number: 3,
  dice: [],
  sumTxt: '',
  total: 0
}

export default {
  name: 'app',
  data: () => {
    return data
  },
  methods: {
    buildDice: () => {
      let dice = []

      for (let i = 0; i < data.number; i++) {
        dice.push(1)
      }

      data.dice = dice
      data.sumTxt = dice.join(' + ')
      data.total = dice.length
    },
    roll: (event) => {
      let dice = data.dice.slice()
      data.total = 0

      for (let i = 0; i < data.number; i++) {
        const newNumber = Math.floor((Math.random() * 6) + 1)
        dice[i] = newNumber
        data.total += newNumber
      }

      data.dice = dice
      data.sumTxt = dice.join(' + ')
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
