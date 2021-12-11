<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->

    <div class="calculatorResultDisplayField">
      {{currentInput}}
    </div>
    <div 
      v-for="(keyRow, index) in keys"
      :key="index"
      class="calculatorRowWrapper">
      <div 
        v-for="(keyInd, index) in keyRow"
        :key="index"
        :class="keyInd.cssClass"
        @click="keyInd.clickHandler">
        <div v-html="keyInd.text"></div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      evaluated: false,
      currentInput: '',
      keys: [
        [
          this.generateKeysObj('Rad', 'greyButton'),
          this.generateKeysObj('Deg', 'greyButton'),
          this.generateKeysObj('x!', 'greyButton'),
          this.generateKeysObj('(', 'greyButton'),
          this.generateKeysObj(')', 'greyButton'),
          this.generateKeysObj('%', 'greyButton'),
          this.generateKeysObj('AC', 'greyButton'),
        ],
        [
          this.generateKeysObj('Inv', 'greyButton'),
          this.generateKeysObj('Sin', 'greyButton'),
          this.generateKeysObj('ln', 'greyButton'),
          this.generateKeysObj('7', 'greyButton', this.handleInput.bind(this, '7')),
          this.generateKeysObj('8', 'greyButton', this.handleInput.bind(this, '8')),
          this.generateKeysObj('9', 'greyButton', this.handleInput.bind(this, '9')),
          this.generateKeysObj('÷', 'greyButton', this.handleInput.bind(this, ' ÷ ')),
        ],
        [
          this.generateKeysObj('π', 'greyButton'),
          this.generateKeysObj('cos', 'greyButton'),
          this.generateKeysObj('log', 'greyButton'),
          this.generateKeysObj('4', 'greyButton', this.handleInput.bind(this, '4')),
          this.generateKeysObj('5', 'greyButton', this.handleInput.bind(this, '5')),
          this.generateKeysObj('6', 'greyButton', this.handleInput.bind(this, '6')),
          this.generateKeysObj('×', 'greyButton', this.handleInput.bind(this, ' x ')),
        ],
        [
          this.generateKeysObj('e', 'greyButton'),
          this.generateKeysObj('tan', 'greyButton'),
          this.generateKeysObj('√', 'greyButton'),
          this.generateKeysObj('1', 'greyButton', this.handleInput.bind(this, '1')),
          this.generateKeysObj('2', 'greyButton', this.handleInput.bind(this, '2')),
          this.generateKeysObj('3', 'greyButton', this.handleInput.bind(this, '3')),
          this.generateKeysObj('-', 'greyButton', this.handleInput.bind(this, ' - ')),
        ],
        [
          this.generateKeysObj('ANS', 'greyButton'),
          this.generateKeysObj('EXP', 'greyButton'),
          this.generateKeysObj('<div>x<sup>y</sup></div>', 'greyButton'),
          this.generateKeysObj('0', 'greyButton', this.handleInput.bind(this, '0')),
          this.generateKeysObj('.', 'greyButton', this.handleInput.bind(this, '.')),
          this.generateKeysObj('=', 'greyButton', this.handleEvaluate.bind(this)),
          this.generateKeysObj('+', 'greyButton', this.handleInput.bind(this, ' + ')),
        ],
      ]
    }
  },
  methods: {
    generateKeysObj (text, cssClass, clickMethod) {
      return {
        text,
        cssClass,
        clickHandler: () => {
          clickMethod();      
        }
      }
    },
    handleEvaluate () {
      this.evaluated = true;
      this.currentInput = eval(this.currentInput);
    },
    handleInput(inputVal) {
      if (this.evaluated) {
        this.currentInput = "";
        this.evaluated = false;
      }
      this.currentInput += `${inputVal}`
    },
    test() {
      console.log("asd");
    }
  }
}
</script>

<style>

.greyButton {
    cursor: pointer;
    line-height: 34px;
    margin: 4px;
    /* box-sizing: border-box; */
    /* position: relative; */
    border-radius: 4px !important;
    background: #dadce0;
    color: #202124;
    border: 1px solid #f1f3f4;
    width: 85px;
}

.calculatorRowWrapper {
    display: flex;
    font-family: Arial, sans-serif;
    text-align: center;
    justify-content: center;
}
.calculatorResultDisplayField {
    height: 72px;
    border: 1px solid #ebebeb;
    border-radius: 8px;
    margin: auto;
    padding: 10px 14px 0 10px;
    box-sizing: border-box;
}
#app {
  /* font-family: Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center;
  color: #2c3e50;
  margin-top: 60px; */
}
</style>
