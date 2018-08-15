<template>
  <div class="calculator">
    <div class="display">{{ displayValue }}</div>
    
    <div type="button" class="btn" @click="clear()">C</div>
    <div type="button" class="btn" @click="changeSign()">+/-</div>
    <div type="button" class="btn" @click="percent()">%</div>
    <div type="button" class="btn operator">÷</div>
    <div type="button" class="btn">7</div>
    <div type="button" class="btn">8</div>
    <div type="button" class="btn">9</div>
    <div type="button" class="btn operator">*</div>
    <div type="button" class="btn">4</div>
    <div type="button" class="btn">5</div>
    <div type="button" class="btn">6</div>
    <div type="button" class="btn operator">-</div>
    <div type="button" class="btn">1</div>
    <div type="button" class="btn">2</div>
    <div type="button" class="btn">3</div>
    <div type="button" class="btn operator">+</div>
    <div type="button" class="btn" style="grid-column: 1 / 3" >0</div>
    <div type="button" class="btn">.</div>
    <div type="button" class="btn operator">=</div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  
  data () {
    return {
      displayValue: 10
    }
  },

  methods: {
    clear () {
      this.$data.displayValue = 0
    },

    changeSign () {
      this.$data.displayValue *= -1
    },

    percent () {
      this.$data.displayValue /= 100
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

