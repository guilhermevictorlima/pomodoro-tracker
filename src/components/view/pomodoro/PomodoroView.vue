<template>
	<main class="ligth-mode">
    <div class="pomodoro-view">
		<span>
			<h1 class="pomodoro-view__title">
        Pomodoro <br/> Tracker
      </h1>
		</span>
      <div class="pomodoro-view__tracker">
        <TemporizadorComponent
            :tempoDoCiclo="obterTempoEmMinutos(workTime)"
        />
      </div>

      <div class="pomodoro-view__container-options">
        <div>
          <InputTempoComponent
              label-text="Work time"
              placeholder="25"
              @aoDigitar="setarWorkTime($event)"
          />
        </div>
        <br/>
        <div class="pomodoro-view__container-options__break-options">
          <InputTempoComponent
              label-text="Short break"
              placeholder="5"
              @aoDigitar="setarShortBreakTime($event)"
          />

          &nbsp;
          &nbsp;
          &nbsp;

          <InputTempoComponent
              label-text="Long break"
              placeholder="15"
              @aoDigitar="setarLongBreakTime($event)"
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
  }
});
</script>

<style>

main {
  height: 100vh;
  background-color: var(--bg-primario);
}

main.ligth-mode {
  --bg-primario: #cf3a3a;
  --bg-secundario: #F47C7C;
  --bg-tarefa-rodando: #8c1a14;
  --bg-pausa-rodando: #8c1a14;

  --cor-texto-input: #DBC8AC;
  --cor-borda-input: #660b0b;
  --cor-placeholder-input: #9c8f7b;
}

main.dark-mode {
  --bg-primario: #774360;
  --bg-secundario: #4C3A51;
  --titulo: #E7AB79;
  --texto: #FFF2F2;
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