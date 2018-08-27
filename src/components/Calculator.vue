<template>
  <div>
    <div class="calculator">
      <div class="display">{{ displayText || 0 }}</div>

      <div type="button" class="btn" @click="clear()">C</div>
      <div type="button" class="btn" @click="changeSign()">+/-</div>
      <div type="button" class="btn" @click="percent()">%</div>
      <div type="button" class="btn operator" @click="divide()">÷</div>
      <div type="button" class="btn" @click="append('7')">7</div>
      <div type="button" class="btn" @click="append('8')">8</div>
      <div type="button" class="btn" @click="append('9')">9</div>
      <div type="button" class="btn operator" @click="multiply()">*</div>
      <div type="button" class="btn" @click="append('4')">4</div>
      <div type="button" class="btn" @click="append('5')">5</div>
      <div type="button" class="btn" @click="append('6')">6</div>
      <div type="button" class="btn operator" @click="subtract()">-</div>
      <div type="button" class="btn" @click="append('1')">1</div>
      <div type="button" class="btn" @click="append('2')">2</div>
      <div type="button" class="btn" @click="append('3')">3</div>
      <div type="button" class="btn operator" @click="add()">+</div>
      <div type="button" class="btn" style="grid-column: 1 / 3" >0</div>
      <div type="button" class="btn" @click="appendDot()">.</div>
      <div type="button" class="btn operator" @click="equal()">=</div>
    </div>

    <div>
      <span>
        Current value: {{ this.$data.displayText }}<br />
        Previous value: {{ this.$data.previous }}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',

  data () {
    return {
      displayText: null,
      previous: null, // Holds paramter a for operator()
      operator: null, // To hold callback functions for operators

      operatorClicked: false
    }
  },

  methods: {
    clear () {
      this.$data.displayText = ''
    },

    changeSign () {
      if (this.$data.displayText) {
        this.$data.displayText = this.$data.displayText.charAt(0) === '-'
          ? this.$data.displayText.substr(1)
          : `-${this.$data.displayText}`
      } // else: Do nothing
    },

    percent () {
      this.$data.displayText = `${parseFloat(this.$data.displayText / 100)}`
    },

    append (number) {
      if (this.$data.operatorClicked) {
        this.clear()
      } // else: Do nothing

      this.$data.displayText = this.$data.displayText === null ? `${number}` : `${this.$data.displayText}${number}`
    },

    appendDot () {
      if (this.$data.displayText.indexOf('.') === -1) {
        this.append('.')
      } // else: Do nothing
    },

    setPrevious () {
      this.$data.previous = this.$data.displayText
      this.$data.operatorClicked = true // THis may be useful for something else...
    },

    divide () {
      this.operator = (a, b) => a / b
      this.setPrevious()
    },

    multiply () {
      this.operator = (a, b) => a * b
      this.setPrevious()
    },

    subtract () {
      this.operator = (a, b) => a - b
      this.setPrevious()
    },

    add () {
      this.operator = (a, b) => a + b
      this.setPrevious()
    },

    equal () {
      this.$data.displayText = `${this.$data.operator(parseFloat(this.$data.previous), parseFloat(this.$data.displayText))}`
      this.$data.previous = null
      this.$data.operatorClicked = false
    }
  },

  filter: {
    // String -> Number
    toNumber(payload) {
      return Number(payload)
    }
  }
}
</script>

<style>
.calculator {
  width: 400px;
  margin: 0 auto;
  font-size: 40px;
  /* 4 columns with infinite rows at 50px tall */
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

.display {
  height: 80px;
  padding: 0 8px;
  text-align: right;
  line-height: 2.5;
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
}

.btn {
  background-color: #EEE;
  border: 1px solid #999;
}

.operator {
  background-color: orange;
  color: white;
}
</style>

