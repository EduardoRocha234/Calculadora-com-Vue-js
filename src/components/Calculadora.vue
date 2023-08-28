<template>
	<div style="display: flex; justify-content: center; align-items: center">
		<div class="calculator">
			<div class="telaDaCalculadora">
				{{ typeValue || '0' }} {{ operatorSimbol }}
			</div>
			<div
				v-for="(button, idx) in buttons"
				:key="idx"
				@click="button.func"
        :class="button.class"
			>
				{{ button.text }}
			</div>
		</div>
	</div>
</template>

<script lang="ts" setup>
import {ref} from 'vue'

const buttons = [
  {text: 'C', func: () => clear(), class: 'button'},
  {text: '+/-', func: () => minusSign(), class: 'operator'},
  {text: '%', func: () => percentage(), class: 'operator'},
  {text: '÷', func: () => divide(), class: 'operator'},
  {text: '7', func: () => addNumber('7'), class: 'button'},
  {text: '8', func: () => addNumber('8'), class: 'button'},
  {text: '9', func: () => addNumber('9'), class: 'button'},
  {text: 'X', func: () => mult(), class: 'operator'},
  {text: '4', func: () => addNumber('4'), class: 'button'},
  {text: '5', func: () => addNumber('5'), class: 'button'},
  {text: '6', func: () => addNumber('6'), class: 'button'},
  {text: '-', func: () => subtract(), class: 'operator'},
  {text: '1', func: () => addNumber('1'), class: 'button'},
  {text: '2', func: () => addNumber('2'), class: 'button'},
  {text: '3', func: () => addNumber('3'), class: 'button'},
  {text: '+', func: () => add(), class: 'operator'},
  {text: '0', func: () => addNumber(0), class: 'zero'},
  {text: '.', func: () => point(), class: 'button'},
  {text: '=', func: () => result(), class: 'operator'},
]

const typeValue = ref<string>('')
const previousValue = ref<string>('')
const clickOperator = ref<boolean>(false)
const calc = ref<(num1: number, num2: number) => number>()
const operatorSimbol = ref<string>('')

const clear = (): void => {
	typeValue.value = ''
	operatorSimbol.value = ''
}

const addNumber = (newNumber: number | string): void => {
	if (clickOperator.value) {
		typeValue.value = ''
		clickOperator.value = false
	}

	typeValue.value = `${typeValue.value}${newNumber}`
}

const minusSign = (): void => {
	//verifica se o primeiro inice  tem o sinal se tiver retorna o segundo indice
	//que seria o número q está a frente do sinal, se n tiver ele retorna com o sinal
	typeValue.value =
		typeValue.value.charAt(0) === '-'
			? typeValue.value.slice(1)
			: `-${typeValue.value}`
}

//ele verifica se á o ponto na operação abaixo pois indexOf retorna -1 caso o mesmo
//não esteja presente. sendo assim se for iggual a -1 significa que não á ponto
//e então ele adiciona o ponto a tela da calculator
const point = (): void => {
	if (typeValue.value.indexOf('.') === -1) {
		addNumber('.')
	}
}

const percentage = (): void => {
	if (!!typeValue.value) {
		typeValue.value = `${parseFloat(typeValue.value) / 100}`
	}
}

//operação para quando clicar o botão o valor atual vire o valor antigo para
//que tenha os dois valores para a operação
const setValue = (): void => {
	previousValue.value = typeValue.value
	clickOperator.value = true
}

const divide = (): void => {
	operatorSimbol.value = '÷'
	calc.value = (num1, num2) => num1 / num2
	setValue()
}

const mult = (): void => {
	operatorSimbol.value = 'x'
	calc.value = (num1, num2) => num1 * num2
	setValue()
}

const subtract = (): void => {
	operatorSimbol.value = '-'
	calc.value = (num1, num2) => num1 - num2
	setValue()
}

const add = (): void => {
	operatorSimbol.value = '+'
	calc.value = (num1, num2) => num1 + num2
	setValue()
}

const result = (): void => {
	operatorSimbol.value = ''
	if (calc.value) {
		const calcResult: number = calc.value(
			parseFloat(previousValue.value),
			parseFloat(typeValue.value)
		)
		typeValue.value = `${calcResult}`
		previousValue.value = ''
	}
}
</script>

<style scoped>
.calculator {
	display: grid;
	grid-template-columns: repeat(4, 1fr);
	grid-auto-rows: auto;
	width: 400px;
	border-radius: 30px;
	box-shadow: 30px rgb(0, 0, 0);
	background: rgba(14, 187, 144, 0.945);
	padding-bottom: 5px;
}

.telaDaCalculadora {
	height: 350px;
	width: 357px;
	border-radius: 25px 25px 10px 10px;
	grid-column: 1 / 5;
	background-color: rgb(8, 230, 163);
	color: rgb(0, 0, 0);
	text-align: center;
	padding: 15px;
	margin-bottom: 2px;
	margin-top: 2px;
	margin-left: 4px;
	border-color: rgb(18, 136, 106);
	border: 2px solid rgb(47, 121, 102);
	height: 50px;
	font-size: 40px;
}
.zero {
	font-weight: bolder;
	height: 45px;
	width: 157px;
	border-radius: 48px;
	font-size: 35px;
	padding: 22px 15px 15px 15px;
	margin: auto;
	grid-column: 1 / 3;
	text-align: center;
	border: 1px solid rgb(35, 58, 49);
	background: rgb(177, 255, 232);
	color: black;
}
.zero:hover {
	background-color: rgb(137, 243, 213);
}

.button {
	height: 48px;
	width: 48px;
	border-radius: 24px;
	font-size: 30px;
	font-weight: bolder;
	display: flex;
	text-align: center;
	justify-content: center;
	padding: 18px 15px 15px 18px;
	border: 1px solid rgb(0, 0, 0);
	background: rgb(177, 255, 236);
	color: black;
	cursor: pointer;
	margin: auto;
}

.button:hover {
	background-color: rgb(137, 243, 213);
}
.operator {
	margin: auto;
	height: 48px;
	width: 48px;
	border-radius: 24px;
	color: #cbdbcf;
	padding: 18px 15px 15px 18px;
	font-weight: bold;
	text-align: center;
	font-size: 30px;
	border: 1px solid rgb(57, 92, 78);
	background: rgb(18, 136, 106);
}

.operator:hover {
	background: rgb(47, 121, 102);
}
</style>
