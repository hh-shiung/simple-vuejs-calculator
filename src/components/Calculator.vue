<template>
  <div class="calculator">
    <div class="display">{{ displayValue ? displayValue : 0 }}</div>
    
    <div type="button" class="btn" @click="clear()">{{ displayValue ? 'C' : 'AC' }}</div>
    <div type="button" class="btn" @click="changeSign()">+/-</div>
    <div type="button" class="btn" @click="percent()">%</div>
    <div type="button" class="btn operator" @click="setOperator('division')">÷</div>
    <div type="button" class="btn" @click="append('7')">7</div>
    <div type="button" class="btn" @click="append('8')">8</div>
    <div type="button" class="btn" @click="append('9')">9</div>
    <div type="button" class="btn operator" @click="setOperator('multiplication')">&times;</div>
    <div type="button" class="btn" @click="append('4')">4</div>
    <div type="button" class="btn" @click="append('5')">5</div>
    <div type="button" class="btn" @click="append('6')">6</div>
    <div type="button" class="btn operator" @click="setOperator('subtraction')">-</div>
    <div type="button" class="btn" @click="append('1')">1</div>
    <div type="button" class="btn" @click="append('2')">2</div>
    <div type="button" class="btn" @click="append('3')">3</div>
    <div type="button" class="btn operator" @click="setOperator('addition')">+</div>
    <div type="button" class="btn" style="grid-column: 1 / 3" @click="appendZero()">0</div>
    <div type="button" class="btn" @click="appendDot()">.</div>
    <div type="button" class="btn operator" @click="calculate()">=</div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  
  data () {
    return {
      displayValue: null,
      holdingValue: null,
      operator: null,
      hasOperator: false, // Reset displayValue for next setOperator value
      pendingReset: false
    }
  },

  methods: {
    clear () {
      this.$data.displayValue = null
      this.$data.holdingValue = null
      this.$data.operator = null
      this.$data.hasOperator = false
      this.$data.pendingReset = false
    },

    hasValue () {
      return Boolean(this.$data.displayValue)
    },

    changeSign () {
      if (this.hasValue()) {
        if (this.$data.displayValue.startsWith('-')) {
          this.$data.displayValue = this.$data.displayValue.slice(1)
        } else {
          this.$data.displayValue = `-${this.$data.displayValue}`
        }
      } // else: Do nothing
    },

    percent () {
      if (this.hasValue()) {
        this.$data.displayValue = `${(parseFloat(this.$data.displayValue) / 100).toFixed(2)}`
      }
    },

    // payload: String
    append (payload) {
      if (this.$data.pendingReset) {
        this.clear()
        this.$data.displayValue = `${payload}`
      } else if (this.$data.hasOperator) {
        this.$data.displayValue = `${payload}`
        this.$data.hasOperator = false
      } else {
        this.$data.displayValue = `${this.$data.displayValue || ''}${payload}`
      }
    },

    appendZero () {
      if (
        this.hasValue() && 
        this.$data.displayValue !== '0'
      ) {
        this.append('0')
      } // else: Do nothing
    },

    appendDot() {
      if (this.hasValue()) {
        if (!this.$data.displayValue.includes('.')) {
          this.append('.')
        } // else: Do nothing
      } // else: Do nothing
    },

    // payload: String
    setOperator(payload) {
      this.$data.hasOperator = true
      this.$data.holdingValue = this.$data.displayValue

      switch(payload) {
        case 'addition':
          this.$data.operator = (a, b) => {
            return `${parseFloat(a) + parseFloat(b)}`
          }
        break
        case 'subtraction':
          this.$data.operator = (a, b) => {
            return `${parseFloat(a) - parseFloat(b)}`
          }
        break
        case 'multiplication':
          this.$data.operator = (a, b) => {
            return `${parseFloat(a) * parseFloat(b)}`
          }
        break
        case 'division':
          this.$data.operator = (a, b) => {
            return `${parseFloat(a) / parseFloat(b)}`
          }
        break
        default:
          console.warn('Invalid operator!')
        break
      }
    },

    calculate () {
      if (this.$data.operator) {
        this.$data.displayValue = `${this.$data.operator(this.$data.holdingValue, this.$data.displayValue)}`
        // this.$data.operator = null
        this.$data.pendingReset = true
      } else {
        // else: Notify that there are no operators set
        console.warn('There are no operators!')
      }
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

