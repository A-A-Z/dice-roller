<template>
  <main id="app">
    <header>
      <h1>test</h1>
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
  </main>
</template>

<script>

let data = {
  number: 3,
  dice: []
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
    },
    roll: (event) => {
      let dice = data.dice.slice()

      for (let i = 0; i < data.number; i++) {
        dice[i] = Math.floor((Math.random() * 6) + 1)
      }

      data.dice = dice
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
