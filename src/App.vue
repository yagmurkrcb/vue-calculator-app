<template>
  <main>
    <div class="calculator">

      <TopComponent v-model="calculationString" />
      <ValueComponent v-model="result" />

      <div class="buttons-container">
        <div @click="ac" class="button ac">AC</div>
        <div class="button function pm">±</div>
        <div class="button function percent">%</div>
        <div class="button operator division">÷</div>
        <div class="button number">7</div>
        <div class="button number">8</div>
        <div class="button number">9</div>
        <div class="button operator multiplication">x</div>
        <div class="button number">4</div>
        <div class="button number">5</div>
        <div class="button number">6</div>
        <div class="button operator subtraction">-</div>
        <div class="button number">1</div>
        <div class="button number">2</div>
        <div class="button number">3</div>
        <div class="button operator addition">+</div>
        <div class="button number number-0">0</div>
        <div @click="dot" class="button decimal">.</div>
        <div class="button operator equal">=</div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import TopComponent from '../src/components/TopComponent.vue'
import ValueComponent from '../src/components/ValueComponent.vue'

const result = ref(0)
const calculationString = ref('')
const savedValue = ref(0)
const savedOperator = ref('')

function ac() {
  result.value = 0
  savedValue.value = 0
  calculationString.value = ''
  savedOperator.value = ''
}

function dot() {
  result.value = result.value + '.'
}

function calculate() {
  let calculationResult = eval(`${savedValue.value} ${savedOperator.value} ${result.value}`)

  calculationString.value = savedValue.value + ` ${savedOperator.value} ` + result.value
  savedValue.value = calculationResult
  result.value = calculationResult
}

onMounted(() => {
  const numberElements = document.getElementsByClassName('number')
  for (let i = 0; i < numberElements.length; i++) {
    numberElements[i].addEventListener('click', (event) => {
      if (result.value === savedValue.value) {
        result.value = event.target.textContent
        return
      }

      if (result.value === 0) {
        result.value = event.target.textContent
      } else {
        result.value += event.target.textContent
      }
    });
  }

  const operatorElements = document.getElementsByClassName('operator')
  for (let i = 0; i < operatorElements.length; i++) {
    operatorElements[i].addEventListener('click', (event) => {
      switch (event.target.textContent) {
        case 'x':
          savedOperator.value = '*'
          break
        case '+':
          savedOperator.value = '+'
          break
        case '-':
          savedOperator.value = '-'
          break
        case '÷':
          savedOperator.value = '/'
          break
        case '=':
          if (savedOperator.value === '') {
            return
          }

          calculate()
          savedOperator.value = ''
          break
        default:
          return
      }

      if (savedOperator.value !== '') {
        if (result.value !== 0 && savedValue.value !== 0) {
          calculate()
          return
        }

        savedValue.value = result.value
        result.value = 0
      }

    });
  }

  const functionElements = document.getElementsByClassName('function')
  for (let i = 0; i < functionElements.length; i++) {
    functionElements[i].addEventListener('click', (event) => {
      switch (event.target.textContent) {
        case '±':
          result.value = parseFloat(result.value) * -1
          break
        case '%':
          result.value = parseFloat(result.value) / 100
          break
        default:
          return
      }
    });
  }
})


</script>

<style scoped></style>
