<template>
  <div id="app">
    <h1>BINGO</h1>
    <p class="pickNumber">{{targetNum}}</p>
    <input v-if="mode === 'stop'" type="button" value="spin" @click="spin()">
    <input v-else-if="mode === 'spin'" type="button" value="stop" @click="stop()">
    <input v-else type="button" value="reset" @click="reset()">
    <ul class="panel">
      <li v-for="num in bingoMaxNum" :class="[activeClass.white, `number-${num}`]" :id="[num]" :key="num">{{ num }}</li>
    </ul>
    <div class="bingoCard">
      <input type="button" value="create carde" @click="createBingoCard()">
      <div class="bingoCard_contents">
        <table>
          <thead class="bingo_head">
            <tr v-for="(head, index) in bingoCardNumber" :key="index"><th>{{ head.title }}</th></tr>
          </thead>
          <tbody class="bingo_body">
            <tr class="bingo_num" v-for="(body, index) in bingoCardNumber" :key="index">
              <td v-for="(num, index) in body.number" :key="index" :class="[activeClass.white, `number-${num}`]">{{ num }}</td>
            </tr>
          </tbody>
        </table>
      </div>
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

    },
    createBingoNumber: function() {
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
    createBingoCard: function() {
      this.createBingCardNumber()
      console.log(this.bingoCardNumber)
      this.choiceNumber()
    },
    createBingCardNumber: function() {
      const sliceNum = this.bingoMaxNum / 5
      const bingoStringSplit = this.bingoString.split('')
      let idx = 0;
      let pickNum = 5
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
  margin-top: 60px;
}
ul {
  list-style: none;
  padding: 0;
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

.bingo_num {
  display: flex;
  flex-direction: column;
}

.bingo_head,
.bingo_body {
  display: flex;
}
</style>
