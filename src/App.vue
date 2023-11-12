<script setup lang="ts">
import { ref } from 'vue';

const state = ref<'stopped' | 'running' | 'paused'>('stopped');
const timeElapsed = ref(0);
const interval = ref<number | undefined>(undefined);


function start() {
	state.value = 'running';
	interval.value = setInterval(() => {
		timeElapsed.value++;
	}, 1000);
};

function pause() {
	state.value = 'paused';
	clearInterval(interval.value);
	interval.value = undefined;
};

function clear() {
	state.value = 'stopped';
	timeElapsed.value = 0;
	if (interval.value !== undefined) {
		clearInterval(interval.value);
		interval.value = undefined;
	};
}

function formatTime(elapsedTime: any) {
	const minutes = `0${Math.floor(elapsedTime / 60) + ''}`.slice(-2);
	const seconds = `0${elapsedTime % 60 + ''}`.slice(-2);
	return `${minutes}:${seconds}`;
}

</script>

<template>
	<div>
		<div>{{ formatTime(timeElapsed) }}</div>
		<button v-if="state === 'stopped'" @click="start()">Старт</button>
		<button v-if="state === 'running'" @click="pause()">Стоп</button>
		<button v-if="state === 'paused'" @click="start()">Продолжить</button>
		<button v-if="state === 'paused'" @click="clear()">Сброс</button>
	</div>
</template>

<style scoped></style>
