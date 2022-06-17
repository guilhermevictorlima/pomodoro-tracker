<template>
	<div class="text-center">
		<span>
			<h1>Pomodoro</h1>
			<h1>Tracker</h1>
		</span>
		<div>
			<TemporizadorComponent :tempoDoCiclo="120" :isTemporizadorRodando="isTemporizadorRodando"/>
			<button @click="isTemporizadorRodando = true">Oi</button>
		</div>
	</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import TemporizadorComponent from '@/components/shared/temporizador/TemporizadorComponent.vue';

export default defineComponent({
	name: 'PomodoroView',
	
	components: {
		TemporizadorComponent
	},

	data() {
		return {
			isTemporizadorRodando: false
		}
	},

});
</script>

<style>

</style>