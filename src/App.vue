<script setup lang="ts">
import Calculator from './components/Calculator.vue'

const mediaLinks = [
	{social: 'GitHub', url: 'https://github.com/EduardoRocha234'},
	{social: 'Instagram', url: 'https://www.instagram.com/eduardoraleixo/'},
	{
		social: 'Twitter',
		url: 'https://twitter.com/EduardoRochaAl7?t=uKabqUZLxCZlQ-zHLvquBA&s=09',
	},
	{
		social: 'Linkedin',
		url: 'https://www.linkedin.com/in/eduardo-rocha-aleixo-068969227/',
	},
]
</script>

<template>
	<div>
		<a
			href="https://vitejs.dev"
			target="_blank"
		>
			<img
				src="/vite.svg"
				class="logo"
				alt="Vite logo"
			/>
		</a>
		<a
			href="https://vuejs.org/"
			target="_blank"
		>
			<img
				src="./assets/vue.svg"
				class="logo vue"
				alt="Vue logo"
			/>
		</a>
	</div>
	<Calculator />

	<div class="midia">
		<h4>About Dev:</h4>
		<a
			v-for="(socialMedia, idx) in mediaLinks"
			:key="idx"
			:href="socialMedia.url"
			target="_blank"
			rel="noopener noreferrer"
			>{{ socialMedia.social }}</a
		>
	</div>
</template>

<style scoped>
.logo {
	height: 6em;
	padding: 1.5em;
	will-change: filter;
	transition: filter 300ms;
}
.logo:hover {
	filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
	filter: drop-shadow(0 0 2em #42b883aa);
}

.midia {
	display: flex;
	align-items: center;
	gap: 1rem;
	position: fixed;
	bottom: 0;
	left: 0;
	padding: 1rem;
	z-index: 999;
}

@media screen and (max-width: 768px) {
	.midia {
		position: relative;
		bottom: auto;
		left: auto;
		padding: 10px;
		width: 100%;
	}
}
</style>
