<template>
	<main class="ligth-mode"
        :class="classeTemporizadorRodando"
  >
    <div class="pomodoro-view">
		<span>
			<h1 class="pomodoro-view__title">
        Pomodoro <br/> Tracker
      </h1>
		</span>
      <div class="pomodoro-view__tracker">
        <TemporizadorComponent
            @temporizadorAtivo="temporizadorAtivo = $event"
            @cicloDeTrabalhoIniciado="isCicloDeTrabalho = $event"
            :disabled-buttons="valorInvalidoInput"
            :work-time="obterTempoEmMinutos(workTime)"
            :short-break-time="obterTempoEmMinutos(shortBreakTime)"
            :long-break-time="obterTempoEmMinutos(longBreakTime)"
        />
      </div>

      <div class="pomodoro-view__container-options">
        <div>
          <InputTempoComponent
              label-text="Work time"
              placeholder="25"
              :disabled="temporizadorAtivo"
              @digitar="setarWorkTime($event)"
              @valorInvalido="valorInvalidoInput = $event"
          />
        </div>
        <br/>
        <div class="pomodoro-view__container-options__break-options">
          <InputTempoComponent
              label-text="Short break"
              placeholder="5"
              :disabled="temporizadorAtivo"
              @digitar="setarShortBreakTime($event)"
              @valorInvalido="valorInvalidoInput = $event"
          />

          &nbsp;
          &nbsp;
          &nbsp;

          <InputTempoComponent
              label-text="Long break"
              placeholder="15"
              :disabled="temporizadorAtivo"
              @digitar="setarLongBreakTime($event)"
              @valorInvalido="valorInvalidoInput = $event"
          />
        </div>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import TemporizadorComponent from '@/components/shared/temporizador/TemporizadorComponent.vue';
import InputTempoComponent from '@/components/shared/inputtempo/InputTempoComponent.vue';

export default defineComponent({
	name: 'PomodoroView',
	
	components: {
		TemporizadorComponent,
    InputTempoComponent,
  },

  data() {
    return {
      workTime: 0,
      shortBreakTime: 0,
      longBreakTime: 0,
      temporizadorAtivo: false,
      isCicloDeTrabalho: false,
      valorInvalidoInput: false
    }
  },

  methods: {
    setarWorkTime(event: string): void {
      this.workTime = parseInt(event);
    },

    setarShortBreakTime(event: string): void {
      this.shortBreakTime = parseInt(event);
    },

    setarLongBreakTime(event: string): void {
      this.longBreakTime = parseInt(event);
    },

    obterTempoEmMinutos(tempoEmSegundos: number): number {
      return tempoEmSegundos * 60;
    }
  },

  computed: {
    classeTemporizadorRodando(): string {
      return this.temporizadorAtivo && this.isCicloDeTrabalho ? 'work-timer-running' : '';
    }
  }
});
</script>

<style>

main {
  height: 100vh;
  background-color: var(--bg-primario);
  transition: 0.3s background-color;
}

.work-timer-running {
  background-color: var(--bg-tarefa-rodando);
}

.pomodoro-view  {
  display: flex;
  flex-direction: column;
  text-align: center;
}

.pomodoro-view__title {
  padding-top: 2rem;
  color: white;
}

.pomodoro-view__tracker {
  margin-top: 5rem;
}

.pomodoro-view__container-options {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 3rem;
}

.pomodoro-view__container-options__break-options {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

</style>