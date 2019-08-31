<template>
  <div id="app">
    <h1>BINGO</h1>
    <bingo-button :count="count" :buttonText="buttonText" @action="action"></bingo-button>
    <Panel :target="target" :bingo="bingo" :activeClass="activeClass"></Panel>
  </div>
</template>

<script>
import Panel from "./components/Panel.vue"
import BingoButton from "./components/BingoButton.vue"

export default {
  name: 'app',
  components: {
    Panel,
    BingoButton
  },
  data(){
    return {
      count: 0,
      target: null,
      bingo: 75,
      activeClass: {
        white: "white",
        black: "black"
      },
      buttonText: {
        start: "start",
        reset: "reset"
      }
    }
  },
  methods: {
    action: function() {
      if (this.count === this.bingo) {
        this.resetBingo();
        return
      }
      const num = this.targetNum();
      const targetEl = document.getElementById(num);

      if (targetEl.classList.contains("white")) {
        this.target = num;
        targetEl.classList.remove(this.activeClass.white);
        targetEl.classList.add(this.activeClass.black);
        this.count++;
      } else {
        this.action();
      }
    },
    targetNum: function() {
      var num = Math.floor( Math.random() * this.bingo + 1);
      return num;
    },
    resetBingo: function() {
      this.count = 0;
      this.target = "";
      for(let i = 1; i < this.bingo + 1; i++) {
        document.getElementById(i).classList.replace(this.activeClass.black, this.activeClass.white)
      }
    }
  },
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
</style>
