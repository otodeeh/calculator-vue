<template>
  <div class="calculator">
    <div class="display">
      <div>{{ value || 0 }}</div>
      <div>{{ history }}</div>
    </div>
    <div class="buttons">
      <button @click="clear()">C</button>
      <button @click="pow()">x²</button>
      <button @click="backspace()">back</button>
      <button @click="divide()">/</button>
      <button @click="parseValue(7)">7</button>
      <button @click="parseValue(8)">8</button>
      <button @click="parseValue(9)">9</button>
      <button @click="mult()">x</button>
      <button @click="parseValue(4)">4</button>
      <button @click="parseValue(5)">5</button>
      <button @click="parseValue(6)">6</button>
      <button @click="sub()">-</button>
      <button @click="parseValue(1)">1</button>
      <button @click="parseValue(2)">2</button>
      <button @click="parseValue(3)">3</button>
      <button @click="sum()">+</button>
      <button disabled>+/-</button>
      <button @click="parseValue(0)">0</button>
      <button @click="dot()">.</button>
      <button @click="equal()">=</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      value: '',
      operator: '',
      operation: null,
      previous: null,
      history: null,
      operationClicked: false
    }
  },
  methods: {
    parseValue(number) {
      if (this.operationClicked) {
        this.value = ''
        this.operationClicked = false
      }
      this.value = `${this.value}${number}`
    },
    dot() {
      if (this.value.indexOf('.') === -1) {
        this.parseValue('.')
      }
    },
    backspace() {
      this.value = this.value.slice(0, -1)
    },
    sum() {
      this.operation = (a, b) => a + b
      this.operator = '+'
      this.setPrevious()
    },
    sub() {
      this.operation = (a, b) => b - a
      this.operator = '-'
      this.setPrevious()
    },
    divide() {
      this.operation = (a, b) => b / a
      this.operator = '/'
      this.setPrevious()
    },
    mult() {
      this.operation = (a, b) => b * a
      this.operator = '*'
      this.setPrevious()
    },
    pow() {
      this.operation = (a, b) => Math.pow(b, a)
      this.operator = '^'
      this.setPrevious()
    },
    setPrevious() {
      if (this.value.slice(-1) === '.') {
        this.backspace()
      }
      this.previous = this.value
      this.operationClicked = true
    },
    equal() {
      if (this.value && this.previous) {
        this.value = `${this.operation(
          parseFloat(this.value),
          parseFloat(this.previous)
        )}`
        this.history = `${this.previous} ${this.operator} ${this.value}`
        this.previous = null
      }
    },
    clear() {
      this.value = ''
      this.previous = null
      this.operation = null
      this.history = null
    }
  }
}
</script>

<style scoped>
.calculator {
  width: 500px;
  border: 1px solid #f2f2f286;
  padding: 5px;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.display {
  width: calc(100% - 10px);
  height: 140px;
  margin: 5px;
  border: 1px solid #f2f2f286;
  border-radius: 8px;
  font-size: 40px;
  padding: 0 10px;
  display: flex;
  flex-direction: column-reverse;
  align-items: flex-end;
}

.buttons {
  display: grid;
  grid-template-columns: 118px 118px 118px 118px;
  grid-template-rows: 70px 70px 70px 70px 70px;
  gap: 2px;
  padding: 5px;
}

button {
  color: #f2f2f2;
  font-size: 30px;
  border-radius: 4px;
  background-color: #f2f2f20e;
  border: 1px solid transparent;
}

button:hover {
  background-color: #f2f2f287;
}

button:disabled {
  background-color: #98989887;
}
</style>
