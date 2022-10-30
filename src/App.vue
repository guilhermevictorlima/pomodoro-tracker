<template>
  <main class="ligth-mode">
    <pomodoro-view/>
  </main>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import PomodoroView from './components/view/pomodoro/PomodoroView.vue';

export default defineComponent({
  name: 'App',
  components: {
    PomodoroView
  }
});
</script>

<style scoped>

main {
  height: 100vh;
  padding-top: 2rem;
  background-color: var(--bg-primario);
}

main.ligth-mode {
  --bg-primario: #cf3a3a;
  --bg-secundario: #F47C7C;
  --bg-tarefa-rodando: #8c1a14;
  --bg-pausa-rodando: #8c1a14;
}

main.dark-mode {
	--bg-primario: #774360;
	--bg-secundario: #4C3A51;
	--titulo: #E7AB79;
	--texto: #FFF2F2;
}

</style>
