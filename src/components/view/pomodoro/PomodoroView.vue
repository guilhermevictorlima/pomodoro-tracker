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
          :isTemporizadorRodando="isTemporizadorRodando"
          @aoFinalizarContagem="isTemporizadorRodando = false"
      />

      <span class="pomodoro-view__tracker-controls--temporizador-inativo" v-if="!isTemporizadorRodando">
        <button @click="isTemporizadorRodando = true" class="btn btn-outline-dark pomodoro-view__tracker-controls__botao">
          <font-awesome-icon icon="fa-solid fa-forward-step"/>
        </button>
      </span>
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

.pomodoro-view__tracker-controls__botao {
  border-radius: 100%;
  background-color: white;
}

.pomodoro-view__tracker-controls__botao:last-child {
  margin-left: 0.5rem;
}

</style>