<template>
    <h1>Calculadora</h1>
    <div class="calculator">
      <div class="display">{{ display }}</div>
      <div class="button-row">
        <button class="button" v-on:click="appendNumber('1')">1</button>
        <button class="button" v-on:click="appendNumber('2')">2</button>
        <button class="button" v-on:click="appendNumber('3')">3</button>
        <button class="button operator" v-on:click="chooseOperation('+')">+</button>
      </div>
      <div class="button-row">
        <button class="button" v-on:click="appendNumber('4')">4</button>
        <button class="button" v-on:click="appendNumber('5')">5</button>
        <button class="button" v-on:click="appendNumber('6')">6</button>
        <button class="button operator" v-on:click="chooseOperation('-')">-</button>
      </div>
      <div class="button-row">
        <button class="button" v-on:click="appendNumber('7')">7</button>
        <button class="button" v-on:click="appendNumber('8')">8</button>
        <button class="button" v-on:click="appendNumber('9')">9</button>
        <button class="button operator" v-on:click="chooseOperation('*')">*</button>
      </div>
      <div class="button-row">
        <button class="button zero" v-on:click="appendNumber('0')">0</button>
        <button class="button operator" v-on:click="chooseOperation('/')">/</button>
      </div>
      <div class="button-row">
        <button class="button operator" v-on:click="clear()">C</button>
        <button class="button operator" v-on:click="compute()">=</button>
      </div>
    </div>
  </template>
    
    <script lang="ts">
    import { defineComponent, ref } from 'vue';
    
    export default defineComponent({
      name: 'Calculator',
      setup() {
        const display = ref('');
        const currentOperand = ref('');
        const previousOperand = ref('');
        const operation = ref<string | null>(null);
    
        const clear = () => {
          display.value = '';
          currentOperand.value = '';
          previousOperand.value = '';
          operation.value = null;
        };
    
        const appendNumber = (number: string) => {
          if (number === '.' && currentOperand.value.includes('.')) return;
          currentOperand.value += number;
          updateDisplay();
        };
    
        const chooseOperation = (op: string) => {
          if (currentOperand.value === '') return;
          if (previousOperand.value !== '') {
            compute();
          }
          operation.value = op;
          previousOperand.value = currentOperand.value;
          currentOperand.value = '';
        };
    
        const compute = () => {
          let computation: number;
          const prev = parseFloat(previousOperand.value);
          const curr = parseFloat(currentOperand.value);
    
          if (isNaN(prev) || isNaN(curr)) return;
    
          switch (operation.value) {
            case '+':
              computation = prev + curr;
              break;
            case '-':
              computation = prev - curr;
              break;
            case '*':
              computation = prev * curr;
              break;
            case '/':
              computation = prev / curr;
              break;
            default:
              return;
          }
    
          currentOperand.value = computation.toString();
          operation.value = null;
          previousOperand.value = '';
          updateDisplay();
        };
    
        const updateDisplay = () => {
          display.value = currentOperand.value;
        };
    
        return {
          display,
          clear,
          appendNumber,
          chooseOperation,
          compute
        };
      }
    });
    </script>
    

    <style scoped>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: linear-gradient(135deg, #f6d365 0%, #fda085 100%);
      margin: 0;
      font-family: Arial, sans-serif;
    }
    
    .calculator {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
      border: 2px solid #fff;
      border-radius: 10px;
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
      background: #fff;
    }
    
    .display {
      width: 100%;
      margin-bottom: 15px;
      padding: 7px;
      background: #333;
      color: #fff;
      font-size: 2em;
      text-align: right;
      border-radius: 5px;
      box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.5);
      height: 60px;
    }
    
    .button-row {
      display: flex;
      justify-content: space-between;
      width: 100%;
      margin-bottom: 10px;
    }
    
    .button {
      flex: 1;
      margin: 5px;
      padding: 15px;
      font-size: 1.5em;
      background: #000000;
      border: none;
      border-radius: 5px;
      box-shadow: 0 5px 10px rgba(0, 0, 0, 0.15);
      cursor: pointer;
      color: white;
      transition: background 0.3s, transform 0.3s;
    }
    
    .button:hover {
      background: #e1e1e1;
      transform: translateY(-3px);
      color: black;
    }
    
    .button:active {
      transform: translateY(2px);
      box-shadow: inset 0 5px 10px rgba(0, 0, 0, 0.2);
    }
    
    .button.operator {
      background: #ff7043;
      color: #fff;
    }
    
    .button.operator:hover {
      background: #ff5722;
      color: black;
    }
    
    .button.zero {
      flex: 2;
    }
    </style>
    