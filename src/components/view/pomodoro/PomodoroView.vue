<template>
	<div class="pomodoro-view">
		<span>
			<h1 class="pomodoro-view__titulo">
        Pomodoro <br/> Tracker
      </h1>
		</span>
		<div class="pomodoro-view__tracker">
			<TemporizadorComponent
          :tempoDoCiclo="120"
      />
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
	}
});
</script>

<style>

.pomodoro-view  {
  display: flex;
  flex-direction: column;
  text-align: center;
}

.pomodoro-view__titulo {
  color: white;
}

.pomodoro-view__tracker {
  margin-top: 5rem;
}

</style>