<template>
  <div id="app">
    <header class="header">
      <h1>BINGO</h1>
    </header>
    <div class="bingo_container">
      <div class="bingo_roulette">
        <p class="pickNumber">{{targetNum}}</p>
        <input v-if="mode === 'stop'" v-bind:disabled="!decision" type="button" class="btn" value="SPIN" @click="spin">
        <input v-else-if="mode === 'spin'" type="button" class="btn" value="STOP" @click="stop">
        <input v-else type="button" class="btn" value="RESET" @click="reset">
        <ul class="panel">
          <li v-for="num in bingoMaxNum" :class="[activeClass.white, `number-${num}`]" :id="[num]" :key="num">{{ num }}</li>
        </ul>
      </div>
      <div class="bingoCard">
        <div class="bingoCard_btn">
          <input v-if="!bingoCardNumber.length" type="button" class="btn" value="CREATE CARD" @click="createBingoCard">
          <input v-else type="button" class="btn" v-bind:disabled="decision" value="CHANGE" @click="createBingoCard">
          <input v-if="bingoCardNumber.length" v-bind:disabled="decision" type="button" class="btn" value="DECIDE" @click="decisionCard">
        </div>
        <div class="bingoCard_content" :class="[!bingoCardNumber.length ? 'is-hide' : '']">
          <table class="bingoCard_table">
            <thead class="bingoCard_head">
              <tr v-for="(head, index) in bingoCardNumber" :key="index"><th>{{ head.title }}</th></tr>
            </thead>
            <tbody class="bingoCard_body">
              <tr class="bingoCard_num" v-for="(body, index) in bingoCardNumber" :key="index">
                <td v-for="(num, index) in body.number" :key="index" :class="[activeClass.white, `number-${num}`]">{{ num }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <div class="resetButton">
      <input type="button" class="btn" value="RESET" @click="reset">
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data(){
    return {
      mode: "stop",
      bingoString: "BINGO",
      target: null,
      targetNum: null,
      bingoMaxNum: 75,
      bingoNum: [],
      bingoCardNumber: [],
      decision: false,
      buttonText: {
        start: "start",
        reset: "reset"
      },
      activeClass: {
        white: "white",
        black: "black"
      },
    }
  },
  methods: {
    spin: function() {
      this.mode = "spin"
      this.activeNumber()
    },
    stop: function() {
      this.mode = "stop"
      this.stopNumber(this.target)
      this.pickNumber()
      this.styleNumber()
      if(!this.bingoNum.length) {
        this.mode = "reset"
      }
    },
    reset: function() {
      this.decision = false
      this.bingoCardNumber = []
      this.targetNum = null
      this.resetStyleNumber()
      this.bingoNum = this.createBingoNumber()
    },
    createBingoNumber: function() {
      // 1からbingoMaxNumまでの配列を作成(1から欲しいので1++)
      return [...Array(this.bingoMaxNum).keys()].map(i => ++i)
    },
    randomNumber: function() {
      return Math.floor(Math.random() * this.bingoNum.length)
    },
    selectNumber: function() {
      const target = this.randomNumber()
      return this.bingoNum[target]
    },
    activeNumber: function() {
      this.target = setInterval(() => {
        this.targetNum = this.selectNumber()
      }, 100);
    },
    stopNumber: function(target) {
      clearInterval(target);
    },
    pickNumber: function() {
      const selectedBingoNum = this.bingoNum.filter((num) => {
        return num !== this.targetNum
      })
      this.bingoNum = selectedBingoNum
    },
    styleNumber: function() {
      const targetElements = document.querySelectorAll(`.number-${this.targetNum}`)
      targetElements.forEach(el => {
        el.classList.remove(this.activeClass.white);
        el.classList.add(this.activeClass.black);
      })
    },
    resetStyleNumber: function() {
      const targetElements = document.querySelectorAll(`.${this.activeClass.black}`)
      targetElements.forEach(el => {
        el.classList.remove(this.activeClass.black);
        el.classList.add(this.activeClass.white);
      })
    },
    createBingoCard: function() {
      this.bingoCardNumber = []
      this.createBingCardNumber()
      this.choiceNumber()
    },
    createBingCardNumber: function() {
      const sliceNum = this.bingoMaxNum / 5
      const bingoStringSplit = this.bingoString.split('')
      const pickNum = 5
      let idx = 0;
      for (let i = 0; i < this.bingoNum.length; i += sliceNum) {
        let splitNumberList = this.bingoNum.slice(i, i + sliceNum)
        let resultNumbers = this.choiceNumber(splitNumberList, pickNum)
        if (idx === 2) {
          resultNumbers.splice(2, 1, "-")
        }
        this.bingoCardNumber.push({
          title: bingoStringSplit[idx],
          number: resultNumbers
        })
        idx++
      }
    },
    choiceNumber: function(numbers, pickNum) {
      const resultNumber = []
      for(let i = 0; i < pickNum; i++) {
        let choiceIdx = Math.floor(Math.random() * numbers.length)
        resultNumber.push(numbers[choiceIdx])
        numbers.splice(choiceIdx, 1)
      }
      return resultNumber
    },
    decisionCard: function() {
      this.decision = true
    }
  },
  created() {
    this.bingoNum = this.createBingoNumber()
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.btn {
  padding: 10px 20px;
  font-weight: bold;
  background-color: #f5f5f5;
  box-shadow: 0px 3px 1px -2px rgba(0, 0, 0, 0.2), 0px 2px 2px 0px rgba(0, 0, 0, 0.14), 0px 1px 5px 0px rgba(0, 0, 0, 0.12);
}

.btn + .btn {
  margin-top: 10px;
}

.header {
  padding: 20px;
  background-color: #333;
}

.header > h1 {
  font-weight: bold;
  color: #fff;
}

ul {
  list-style: none;
  padding: 0;
}

.is-show {
  display: block;
}

.is-hide {
  display: none;
}

.view {
  margin: 20px auto;
  width: 50px;
  height: 50px;
  color: #fff;
  background-color: #333;
  display: flex;
  align-items: center;
  justify-content: center;
}

.pickNumber {
  width: 50px;
  height: 50px;
  margin: 0 auto 20px;
  border: 1px solid #333;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.panel {
  margin: 0 auto;
  width: 500px;
  display: flex;
  flex-wrap: wrap;
}

.panel > li {
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.black {
  color: #fff;
  background-color: #333;
}

.bingo_container {
  display: flex;
  justify-content: space-around;
  max-width: 980px;
  margin: 50px auto;
}

.bingoCard {
  width: 100%;
  max-width: 300px;
}

.bingoCard_btn {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.bingoCard_content {
  margin-top: 30px;
}

.bingoCard_table {
  display: block;
  margin: 0 auto;
  width: 100%;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 4px 10px #ccc;
}

.bingoCard_head,
.bingoCard_body {
  display: flex;
  justify-content: space-between;
}

.bingoCard_head tr {
  width: 60px;
  height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.bingoCard_head th {
  font-weight: bold;
}

.bingoCard_num {
  width: 20%;
  height: 300px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.bingoCard_body td {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.bingoCard_head {
  background-color: #ccc;
}


</style>
