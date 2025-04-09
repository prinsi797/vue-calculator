<template>
  <div class="calculator">
    <div class="display">
      <div class="expression" v-if="expression">{{ expression }}</div>
      <div>{{ current || '0' }}</div>
    </div>

    <div class="buttons">
      <button @click="clear">C</button>
      <button @click="sign">+/-</button>
      <button @click="percent">%</button>
      <button @click="divide" class="operator">÷</button>

      <button @click="append('7')">7</button>
      <button @click="append('8')">8</button>
      <button @click="append('9')">9</button>
      <button @click="multiply" class="operator">×</button>

      <button @click="append('4')">4</button>
      <button @click="append('5')">5</button>
      <button @click="append('6')">6</button>
      <button @click="subtract" class="operator">-</button>

      <button @click="append('1')">1</button>
      <button @click="append('2')">2</button>
      <button @click="append('3')">3</button>
      <button @click="add" class="operator">+</button>

      <button @click="append('0')" class="zero">0</button>
      <button @click="dot">.</button>
      <button @click="equal" class="operator">=</button>

      <!-- Advanced Buttons -->
      <button @click="square">x²</button>
      <button @click="cube">x³</button>
      <button @click="sqrt">√</button>
      <button @click="power" class="operator">xʸ</button>

      <button @click="sin">sin</button>
      <button @click="cos">cos</button>
      <button @click="tan">tan</button>
    </div>
  </div>
</template>


<script>

export default {
  name: 'CalculatorData',
  data() {
    return {
      current: '',
      previous: null,
      operator: null,
      operatorClicked: false,
      expression: ''
    }
  },
  methods: {
    clear() {
      this.current = '';
      this.previous = null;
      this.operator = null;
      this.operatorClicked = false;
      this.expression = '';
    },
    sign() {
      this.current = this.current.charAt(0) === '-'
        ? this.current.slice(1)
        : `-${this.current}`;
    },
    percent() {
      this.current = `${parseFloat(this.current) / 100}`;
    },
    append(number) {
      if (this.operatorClicked) {
        this.current = '';
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`;
    },
    dot() {
      if (this.current.indexOf('.') === -1) {
        this.append('.');
      }
    },
    setPrevious(operatorSymbol) {
      this.previous = this.current;
      this.expression = `${this.current} ${operatorSymbol}`;
      this.operatorClicked = true;
    },
    divide() {
      this.operator = (a, b) => a / b;
      this.setPrevious('÷');
    },
    multiply() {
      this.operator = (a, b) => a * b;
      this.setPrevious('×');
    },
    subtract() {
      this.operator = (a, b) => a - b;
      this.setPrevious('-');
    },
    add() {
      this.operator = (a, b) => a + b;
      this.setPrevious('+');
    },
    power() {
      this.operator = (a, b) => Math.pow(a, b);
      this.setPrevious('^');
    },
    square() {
      this.expression = `sqr(${this.current})`;
      this.current = `${Math.pow(parseFloat(this.current), 2)}`;
    },
    cube() {
      this.expression = `cube(${this.current})`;
      this.current = `${Math.pow(parseFloat(this.current), 3)}`;
    },
    sqrt() {
      this.expression = `√(${this.current})`;
      this.current = `${Math.sqrt(parseFloat(this.current))}`;
    },
    sin() {
      this.expression = `sin(${this.current})`;
      this.current = `${Math.sin(parseFloat(this.current) * Math.PI / 180).toFixed(5)}`;
    },
    cos() {
      this.expression = `cos(${this.current})`;
      this.current = `${Math.cos(parseFloat(this.current) * Math.PI / 180).toFixed(5)}`;
    },
    tan() {
      this.expression = `tan(${this.current})`;
      this.current = `${Math.tan(parseFloat(this.current) * Math.PI / 180).toFixed(5)}`;
    },
    equal() {
      if (this.operator && this.previous) {
        this.current = `${this.operator(
          parseFloat(this.previous),
          parseFloat(this.current)
        )}`;
        this.previous = null;
        this.operator = null;
        this.expression = '';
      }
    }
  }
}
</script>

<style scoped>
.calculator {
  width: 340px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
.expression {
  font-size: 14px;
  color: #888;
  text-align: right;
}
.display {
  background-color: #336699;
  color: white;
  padding: 20px;
  font-size: 24px;
  text-align: right;
  margin-bottom: 20px;
  border-radius: 5px;
  min-height: 30px;
}
.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}
button {
  padding: 15px;
  font-size: 18px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #aec0d3;
  cursor: pointer;
}
button:hover {
  background-color: #f0f0f0;
}
button.operator {
  background-color: #aec0d3;
  color: #ffff;
}
button.zero {
  grid-column: span 2;
}
</style>
