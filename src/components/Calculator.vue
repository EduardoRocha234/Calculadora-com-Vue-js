<template>
	<div>
		<div class="calculator">
			<div
				class="calculatorScreen"
				tabindex="0"
				@keydown="onPress"
				@click="showTip"
			>
				{{ typeValue }}
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
import '../assets/Calculator.css'
import {ref} from 'vue'

const typeValue = ref<string>('')
const viewTip = ref<boolean>(false)

type TButtons = {
	text: string
	func: () => void
	class: string
}

const buttons: TButtons[] = [
	{text: 'C', func: () => clear(), class: 'button'},
	{text: '+/-', func: () => minusSign(), class: 'operator'},
	{text: '%', func: () => addPercentage(), class: 'operator'},
	{text: '÷', func: () => addOperator('÷'), class: 'operator'},
	{text: '7', func: () => addNumber('7'), class: 'button'},
	{text: '8', func: () => addNumber('8'), class: 'button'},
	{text: '9', func: () => addNumber('9'), class: 'button'},
	{text: 'x', func: () => addOperator('x'), class: 'operator'},
	{text: '4', func: () => addNumber('4'), class: 'button'},
	{text: '5', func: () => addNumber('5'), class: 'button'},
	{text: '6', func: () => addNumber('6'), class: 'button'},
	{text: '-', func: () => addOperator('-'), class: 'operator'},
	{text: '1', func: () => addNumber('1'), class: 'button'},
	{text: '2', func: () => addNumber('2'), class: 'button'},
	{text: '3', func: () => addNumber('3'), class: 'button'},
	{text: '+', func: () => addOperator('+'), class: 'operator'},
	{text: '0', func: () => addNumber('0'), class: 'zero'},
	{text: '.', func: () => point(), class: 'button'},
	{text: '=', func: () => result(), class: 'operator'},
]

const onPress = (e: KeyboardEvent) => {
	if (e.key == 'Backspace' && typeValue.value.length > 0) {
		const removeLastChar = typeValue.value.slice(0, -1)
		typeValue.value = removeLastChar
	}

	if (e.key == 'Enter' && typeValue.value.length > 0) {
		result()
	}

	const keyBoard: {[key: string]: string} = {
		'1': '1',
		'2': '2',
		'3': '3',
		'4': '4',
		'5': '5',
		'6': '6',
		'7': '7',
		'8': '8',
		'9': '9',
		'0': '0',
		'+': '+',
		'-': '-',
		'/': '÷',
		'*': 'x',
		'%': '%',
		'.': '.',
	}

	const keyPress = keyBoard[e.key]

	if (keyPress) {
		if ('+-x÷%'.includes(keyPress)) {
			keyPress == '%' ? addPercentage() : addOperator(keyPress)
		} else if (keyPress == '.') {
			point()
		} else {
			addNumber(keyPress)
		}
	}
}

const result = (): void => {
	let strArr: string[] = typeValue.value.split(' ')
	strArr = calcWithPrecedence(strArr)

	if (strArr[0] !== 'NaN') {
		typeValue.value = `${strArr[0]}`
	}
}

const calculate = (arr: string[], operator: string): void => {
	let res: (a: number, b: number) => number
	const calcIfHasPercentage = (a: string): number =>
		a.slice(-1) == '%' ? parseFloat(a) / 100 : parseFloat(a)

	if (operator == 'x') {
		res = (a, b) => a * b
	} else if (operator == '÷') {
		res = (a, b) => a / b
	} else if (operator == '-') {
		res = (a, b) => a - b
	} else {
		res = (a, b) => a + b
	}

	const idx = arr.indexOf(operator)
	const nexIndx = idx + 1
	const prevIndx = idx - 1

	const nextVal = calcIfHasPercentage(arr[nexIndx])
	const prevVal = calcIfHasPercentage(arr[prevIndx])

	const result: number = res(prevVal, nextVal)

	arr.splice(idx, 1, '' + result)
	arr.splice(nexIndx, 1)
	arr.splice(prevIndx, 1)
}

const calcWithPrecedence = (arr: string[]): string[] => {
	if (arr.length == 1 && arr[0].slice(-1) == '%') {
		arr[0] = String(parseFloat(arr[0]) / 100)
	}

	while (arr.includes('x')) {
		calculate(arr, 'x')
	}

	while (arr.includes('÷')) {
		calculate(arr, '÷')
	}

	while (arr.includes('+')) {
		calculate(arr, '+')
	}

	while (arr.includes('-')) {
		calculate(arr, '-')
	}

	return arr
}

const clear = (): void => {
	typeValue.value = ''
}

const addNumber = (newNumber: string): void => {
	typeValue.value += newNumber
}

const addOperator = (newOperator: string): void => {
	const removeSpaces = typeValue.value.trim()

	if (
		removeSpaces.endsWith('+') ||
		removeSpaces.endsWith('-') ||
		removeSpaces.endsWith('x') ||
		removeSpaces.endsWith('÷')
	) {
		const removeActualOperator = removeSpaces.slice(0, removeSpaces.length - 1)
		typeValue.value = removeActualOperator + newOperator + ' '
	} else {
		typeValue.value += ` ${newOperator} `
	}
}

const addPercentage = (): void => {
	const strArr = typeValue.value.split(' ')
	const lastCharacter = strArr[strArr.length - 1]

	const addPercentage =
		lastCharacter.slice(-1) === '%'
			? lastCharacter.slice(0, lastCharacter.length - 1)
			: `${lastCharacter}%`

	strArr.pop()
	strArr.push(addPercentage)

	typeValue.value = strArr.join(' ')
}

const minusSign = (): void => {
	const strArr = typeValue.value.split(' ')
	const lastCharacter = strArr[strArr.length - 1]

	const addMinusSign =
		lastCharacter.charAt(0) === '-'
			? lastCharacter.slice(1)
			: `-${lastCharacter}`

	strArr.pop()
	strArr.push(addMinusSign)

	typeValue.value = strArr.join(' ')
}

const point = (): void => {
	const verifyPoint =
		typeValue.value.split(' ')[typeValue.value.split(' ').length - 1]
	if (verifyPoint.indexOf('.') === -1) {
		typeValue.value += '.'
	} else {
		typeValue.value = typeValue.value.slice(0, -1)
	}
}

const showTip = (): void => {
	if (!viewTip.value) {
		alert(
			'Hello, good to see you, if you prefer you can use the numeric keypad 😉'
		)
		viewTip.value = true
	}
}
</script>
