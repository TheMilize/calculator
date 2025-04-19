<template>
  <div class="calculator">
    <div class="display">
      <div class="operation">{{ operationDisplay }}</div>
      <div class="number">{{ display }}</div>
    </div>
    <div class="main">
      <div class="buttons">
        <div class="button_row">
          <div class="btn">
            <button class="button3" @click="clear">
              <span class="button_text">C</span>
            </button>
          </div>
          <div class="btn">
            <button class="button4" @click="sqrt">
              <span class="button_text">√</span>
            </button>
          </div>
          <div class="btn">
            <button class="button4" @click="square">
              <span class="button_text">x²</span>
            </button>
          </div>
          <div class="btn">
            <button class="button4" @click="percent">
              <span class="button_text">%</span>
            </button>
          </div>
        </div>
        <div class="button_row">
          <div class="btn">
            <button class="button4" @click="add">
              <span class="button_text">+</span>
            </button>
          </div>
          <div class="btn">
            <button class="button4" @click="subtract">
              <span class="button_text">-</span>
            </button>
          </div>
          <div class="btn">
            <button class="button4" @click="multiply">
              <span class="button_text">×</span>
            </button>
          </div>
          <div class="btn">
            <button class="button4" @click="divide">
              <span class="button_text">÷</span>
            </button>
          </div>
        </div>
        <div class="button_row">
          <div class="btn">
            <button class="button1" @click="appendNumber(7)">7</button>
          </div>
          <div class="btn">
            <button class="button1" @click="appendNumber(8)">8</button>
          </div>
          <div class="btn">
            <button class="button1" @click="appendNumber(9)">9</button>
          </div>
          <div class="btn">
            <button class="button3" @click="equals">
              <span class="button_text">=</span>
            </button>
          </div>
        </div>
        <div class="button_row">
          <div class="btn">
            <button class="button1" @click="appendNumber(4)">4</button>
          </div>
          <div class="btn">
            <button class="button1" @click="appendNumber(5)">5</button>
          </div>
          <div class="btn">
            <button class="button1" @click="appendNumber(6)">6</button>
          </div>
          <div class="btn">
            <button class="button1" @click="appendDecimal">.</button>
          </div>
        </div>
        <div class="button_row">
          <div class="btn">
            <button class="button1" @click="appendNumber(1)">1</button>
          </div>
          <div class="btn">
            <button class="button1" @click="appendNumber(2)">2</button>
          </div>
          <div class="btn">
            <button class="button1" @click="appendNumber(3)">3</button>
          </div>
          <div class="btn">
            <button class="button1" @click="appendNumber(0)">0</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data() {
    return {
      display: '0',
      firstNumber: null,
      operator: null,
      waitingForSecondNumber: false,
      operationDisplay: '',
      hasDecimal: false
    }
  },
  methods: {
    clear() {
      this.display = '0'
      this.firstNumber = null
      this.operator = null
      this.waitingForSecondNumber = false
      this.operationDisplay = ''
      this.hasDecimal = false
    },
    appendNumber(number) {
      if (this.waitingForSecondNumber) {
        this.display = number.toString()
        this.waitingForSecondNumber = false
        this.hasDecimal = false
      } else {
        this.display = this.display === '0' ? number.toString() : this.display + number
      }
    },
    appendDecimal() {
      if (!this.hasDecimal) {
        if (this.waitingForSecondNumber) {
          this.display = '0.'
          this.waitingForSecondNumber = false
        } else if (this.display === '0') {
          this.display = '0.'
        } else {
          this.display += '.'
        }
        this.hasDecimal = true
      }
    },
    setOperator(op) {
      this.firstNumber = parseFloat(this.display)
      this.operator = op
      this.waitingForSecondNumber = true
      this.hasDecimal = false
      this.operationDisplay = `${this.firstNumber} ${op}`
    },
    add() {
      this.setOperator('+')
    },
    subtract() {
      this.setOperator('-')
    },
    multiply() {
      this.setOperator('×')
    },
    divide() {
      this.setOperator('÷')
    },
    sqrt() {
      const number = parseFloat(this.display)
      if (number >= 0) {
        const result = Math.sqrt(number)
        this.operationDisplay = `√(${number}) =`
        this.display = result.toString()
      } else {
        this.display = 'Error'
      }
    },
    square() {
      const number = parseFloat(this.display)
      const result = number * number
      this.operationDisplay = `(${number})² =`
      this.display = result.toString()
    },
    percent() {
      const number = parseFloat(this.display)
      const result = number / 100
      this.operationDisplay = `${number}% =`
      this.display = result.toString()
    },
    equals() {
      if (this.operator === null || this.waitingForSecondNumber) return
      
      const secondNumber = parseFloat(this.display)
      let result
      
      switch (this.operator) {
        case '+':
          result = this.firstNumber + secondNumber
          break
        case '-':
          result = this.firstNumber - secondNumber
          break
        case '×':
          result = this.firstNumber * secondNumber
          break
        case '÷':
          result = this.firstNumber / secondNumber
          break
      }
      
      this.operationDisplay = `${this.firstNumber} ${this.operator} ${secondNumber} =`
      this.display = result.toString()
      this.operator = null
      this.waitingForSecondNumber = false
      this.hasDecimal = this.display.includes('.')
    }
  }
}
</script>

<style scoped>
.calculator {
  max-width: 450px;
  width: 90%;
  margin: 0 auto;
  padding: 25px;
  background-color: rgba(44, 44, 44, 0.95);
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
}

.display {
  background-color: rgba(23, 23, 23, 0.95);
  color: white;
  padding: 20px;
  text-align: right;
  margin-bottom: 20px;
  border-radius: 10px;
  font-family: Montserrat;
  min-height: 1.2em;
  overflow: hidden;
  box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.3);
}

.operation {
  font-size: 1.2em;
  color: #888;
  min-height: 1.2em;
  margin-bottom: 5px;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.3);
}

.number {
  font-size: 2.5em;
  color: white;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.3);
}

.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.buttons {
  display: flex;
  flex-direction: column;
  gap: 15px;
  width: 100%;
}

.button_row {
  display: flex;
  justify-content: space-between;
  gap: 15px;
  width: 100%;
}

.btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 80px;
  height: 80px;
  background-color: #171717;
  border-radius: 5px;
}

.button1 {
  width: 100%;
  height: 100%;
  border-radius: 10px;
  border: none;
  outline: none;
  background-color: #c7c3c0;
  box-shadow: rgba(0, 0, 0, 0.377) 10px 10px 8px,
    #ffffff 1.5px 1.5px 2px 0px inset, #c7c3c0 -3.2px -3.2px 8px 0px inset;
  cursor: pointer;
  font-family: Montserrat;
  transition: all 0.1s ease-in-out;
  color: #5f5f5f;
  font-size: 1.5em;
  font-weight: 500;
}

.button2 {
  width: 100%;
  height: 100%;
  border-radius: 10px;
  border: none;
  outline: none;
  background-color: #c7c3c0;
  box-shadow: rgba(0, 0, 0, 0.377) 10px 10px 8px,
    #ffffff 1.5px 1.5px 2px 0px inset, #c7c3c0 -3.2px -3.2px 8px 0px inset;
  cursor: pointer;
  font-family: Montserrat;
  transition: all 0.1s ease-in-out;
}

.button3 {
  width: 100%;
  height: 100%;
  border-radius: 10px;
  border: none;
  outline: none;
  background-color: #d42a02;
  box-shadow: rgba(0, 0, 0, 0.377) 10px 10px 8px, #fb702c 2px 2px 10px 0px inset,
    #d42a02 -4px -4px 1px 0px inset;
  cursor: pointer;
  font-family: Montserrat;
  transition: all 0.1s ease-in-out;
}

.button4 {
  width: 100%;
  height: 100%;
  border-radius: 10px;
  border: none;
  outline: none;
  background-color: #545251;
  box-shadow: rgba(0, 0, 0, 0.377) 10px 10px 8px,
    #a8a6a4 1.5px 1.5px 1px 0px inset, #545251 -3.2px -3.2px 8px 0px inset;
  cursor: pointer;
  font-family: Montserrat;
  transition: all 0.1s ease-in-out;
}

.button_text {
  color: white;
  letter-spacing: 0.075em;
  font-size: 1.5em;
  font-weight: 500;
  transition: all 0.1s ease-in-out;
}

.button1:active,
.button2:active {
  transform: translateY(2px);
  box-shadow: rgba(0, 0, 0, 0.377) 0px 0px 0px, inset 0.5px 0.5px 4px #000000,
    #c7c3c0 -3.2px -3.2px 8px 0px inset;
}

.button3:active {
  transform: translateY(2px);
  box-shadow: rgba(0, 0, 0, 0.377) 0px 0px 0px, inset 0.5px 0.5px 4px #000000,
    #d42a02 -3.2px -3.2px 8px 0px inset;
}

.button4:active {
  transform: translateY(2px);
  box-shadow: rgba(0, 0, 0, 0.377) 0px 0px 0px, inset 0.5px 0.5px 4px #000000,
    #545251 -3.2px -3.2px 8px 0px inset;
}

@media (max-width: 480px) {
  .calculator {
    width: 95%;
    padding: 20px;
  }

  .display {
    padding: 15px;
  }

  .operation {
    font-size: 1em;
  }

  .number {
    font-size: 2em;
  }

  .btn {
    width: 65px;
    height: 65px;
  }

  .button_text {
    font-size: 1.3em;
  }
}

@media (max-width: 360px) {
  .calculator {
    width: 100%;
    padding: 15px;
  }

  .display {
    padding: 12px;
  }

  .operation {
    font-size: 0.9em;
  }

  .number {
    font-size: 1.8em;
  }

  .btn {
    width: 55px;
    height: 55px;
  }

  .button_text {
    font-size: 1.2em;
  }
}
</style> 