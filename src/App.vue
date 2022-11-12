<template>
  <pomodoro-view/>
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

<style>
* {
  --bg-primario: #cf3a3a;
  --bg-tarefa-rodando: #8c1a14;
  --cor-texto-input: #DBC8AC;
  --cor-placeholder-input: #9c8f7b;
  --cor-borda-input: #660b0b;
}
</style>
