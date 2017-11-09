<template>
  <div id="app">
    <img src="./assets/logo.png" id="logo">
    <div class="win" v-if="winBy" @click="restart()"><h2>Game won by {{this.winBy}}</h2>Click to restart</div>
    <div class="grid">
      <div v-for="(block, index) in grid" @click="select(index)" :key="index">
        <block :figure="block.figure"/>
      </div>
    </div>
  </div>
</template>

<script>
import './index.css'
import _ from 'lodash'
import block from './components/Block'

export default {
  name: 'app',
  components: {
    block
  },
  data () {
    return {
      grid: undefined,
      myTurn: false,
      winBy: ''
    }
  },
  computed: {
    winner () {
      const wins = ['012', '036', '345', '147', '258', '678', '048', '246']
      const player = this.myTurn ? 0 : 1
      const moves = _.reduce(this.grid, (result, value, index) => {
        if (value.figure === player) result.push(index)
        return result
      }, [])

      return !!_.find(wins, win => {
        const combination = _.map(win.split(''), n => parseInt(n))
        return _.difference(combination, moves).length === 0
      })
    }
  },
  watch: {
    'winner': function (won) {
      if (won) {
        if (this.myTurn) this.winBy = 'O'
        else this.winBy = 'X'
        console.log('We have a winner: ' + this.winBy)
      }
    }
  },
  created () {
    this.initGame()
  },
  methods: {
    initGame () {
      this.grid = _.map(_.range(0, 9), (index) => {
        return { index, figure: -1 }
      })
      this.winBy = ''
    },
    select (index) {
      if (this.winner) return
      const figure = this.grid[index]
      if (figure > -1) return
      this.grid[index].figure = this.myTurn ? 1 : 0
      this.myTurn = !this.myTurn
    },
    restart () {
      console.log('Restarting game...')
      this.initGame()
    }
  }
}
</script>
