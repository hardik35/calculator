<template>
  <div id="app">
    <div class="calculatorResultDisplayField">
      {{currentInput}}
    </div>
    <div style="margin: 8px 0 0 0; width: 600px">
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
          this.generateKeysObj('Rad', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('Deg', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('x!', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('(', 'greyButton', this.test.bind(this)),
          this.generateKeysObj(')', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('%', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('CE', 'greyButton', this.test.bind(this)),
        ],
        [
          this.generateKeysObj('Inv', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('Sin', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('ln', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('7', 'lightBlueButtons', this.handleInput.bind(this, '7')),
          this.generateKeysObj('8', 'lightBlueButtons', this.handleInput.bind(this, '8')),
          this.generateKeysObj('9', 'lightBlueButtons', this.handleInput.bind(this, '9')),
          this.generateKeysObj('÷', 'greyButton', this.handleInput.bind(this, ' / ', true)),
        ],
        [
          this.generateKeysObj('π', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('cos', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('log', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('4', 'lightBlueButtons', this.handleInput.bind(this, '4')),
          this.generateKeysObj('5', 'lightBlueButtons', this.handleInput.bind(this, '5')),
          this.generateKeysObj('6', 'lightBlueButtons', this.handleInput.bind(this, '6')),
          this.generateKeysObj('×', 'greyButton', this.handleInput.bind(this, ' * ', true)),
        ],
        [
          this.generateKeysObj('e', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('tan', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('√', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('1', 'lightBlueButtons', this.handleInput.bind(this, '1')),
          this.generateKeysObj('2', 'lightBlueButtons', this.handleInput.bind(this, '2')),
          this.generateKeysObj('3', 'lightBlueButtons', this.handleInput.bind(this, '3')),
          this.generateKeysObj('-', 'greyButton', this.handleInput.bind(this, ' - ', true)),
        ],
        [
          this.generateKeysObj('ANS', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('EXP', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('x^y', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('0', 'lightBlueButtons', this.handleInput.bind(this, '0')),
          this.generateKeysObj('.', 'lightBlueButtons', this.handleInput.bind(this, '.')),
          this.generateKeysObj('=', 'blueButton', this.handleEvaluate.bind(this)),
          this.generateKeysObj('+', 'greyButton', this.handleInput.bind(this, ' + ', true)),
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
      try {
        this.currentInput = eval(this.currentInput).toString();
      }
      catch {
        alert("invalid operation reseting calculator")
        this.currentInput = "";
        this.evaluated = false;
      }

    },
    handleInput(inputVal, operator=false) {
      const operators = ["+", "-", "/", "*"]
      if (this.evaluated && !operator && !operators.map((term) => this.currentInput.includes(term)).includes(true)) {
        this.currentInput = "";
        this.evaluated = false;
      }
      this.currentInput += `${inputVal}`
    },
    test() {
      console.log("test");
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

.blueButton {
    background: #4285f4;
    color: #fff;
    border: 1px solid #4285f4;
    border-radius: 4px;
    cursor: pointer;
    width: 85px;
    line-height: 34px;
    margin: 4px;
}

.lightBlueButtons {
    background: #f1f3f4;
    color: #202124;
    border: 1px solid #f1f3f4;
    border-radius: 4px;
    cursor: pointer;
    width: 85px;
    line-height: 34px;
    margin: 4px;
}

.calculatorRowWrapper {
    display: flex;
    font-family: Arial, sans-serif;
    text-align: center;
    justify-content: left;
}
.calculatorResultDisplayField {
    height: 72px;
    border: 1px solid #ebebeb;
    border-radius: 8px;
    /* margin: auto; */
    padding: 10px 14px 0 10px;
    box-sizing: border-box;
    /* width: 70vw;  */
    width: 600px
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
