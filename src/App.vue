<template>
  <div id="app">
    <div class="calculatorResultDisplayField">
      <div v-if="evaluated" class="currentOperationsSet">{{operationsSet}} =</div>
      <div v-if="!evaluated && lastCalculation" class="currentOperationsSet">Ans = {{lastCalculation}}</div>
      <div class="currentInput">{{currentInput}}</div>
      <div class="historyIcon">
        <svg viewBox="0 0 24 24">
          <path 
            d="M13 3a9 9 0 0 0-9 9H1l3.89 3.89.07.14L9 12H6c0-3.87 3.13-7 7-7s7 3.13 7 7-3.13 7-7 7c-1.93 0-3.68-.79-4.94-2.06l-1.42 1.42A8.954 8.954 0 0 0 13 21a9 9 0 0 0 0-18zm-1 5v5l4.28 2.54.72-1.21-3.5-2.08V8H12z">
          </path>
        </svg>
      </div>
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
          <div>{{keyInd.text}}</div>
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
      operationsSet: '',
      lastCalculation: '',
      keys: [
        [
          this.generateKeysObj('Rad', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('Deg', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('x!', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('(', 'greyButton', this.test.bind(this)),
          this.generateKeysObj(')', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('%', 'greyButton', this.test.bind(this)),
          this.generateKeysObj('CE', 'greyButton', this.handleCE.bind(this)),
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
        cssClass: `buttonCommon ${cssClass}`,
        clickHandler: () => {
          clickMethod();      
        }
      }
    },
    handleCE() {
      if (this.evaluated) {
        this.lastCalculation = this.currentInput;
      }
      this.currentInput = "";
      this.evaluated = false;
    },
    handleEvaluate () {
      this.evaluated = true;
      try {
        this.operationsSet = this.currentInput;
        this.currentInput = eval(this.currentInput).toString();
      }
      catch {
        alert("invalid operation reseting calculator")
        this.currentInput = "";
        this.evaluated = false;
        this.operationsSet = '';
      }

    },
    handleInput(inputVal, operator=false) {
      const operators = ["+", "-", "/", "*"];
      const containsOperand = operators.map((term) => this.currentInput.includes(term)).includes(true)
      if (this.evaluated && !operator && !containsOperand) {
        this.lastCalculation = this.currentInput;
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

.buttonCommon {
    cursor: pointer;
    line-height: 34px;
    width: 85px;
    margin: 4px;
    border-radius: 4px !important;
}

.greyButton {
    background: #dadce0;
    color: #202124;
    border: 1px solid #f1f3f4;
}

.blueButton {
    background: #4285f4;
    color: #fff;
    border: 1px solid #4285f4;
}

.lightBlueButtons {
    background: #f1f3f4;
    color: #202124;
    border: 1px solid #f1f3f4;
}

.calculatorRowWrapper {
    display: flex;
    text-align: center;
    justify-content: left;
}
.calculatorResultDisplayField {
    height: 72px;
    border: 1px solid #ebebeb;
    border-radius: 8px;
    padding: 10px 14px 0 10px;
    width: 565px;
    position: relative;
}

.currentInput {
  position: absolute;
  font-size: 30px;
  height: 32px;
  bottom: 10px;
  right: 10px;
}

.historyIcon {
  height: 22px; 
  line-height: 22px; 
  width: 22px; 
  color: #70757a; 
  outline: 0; 
  fill: #70757a
}
.currentOperationsSet {
  position: absolute;
  font-size: 13px;
  right: 10px;
  color: #70757a;
}
#app {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
