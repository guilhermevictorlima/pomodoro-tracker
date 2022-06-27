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

main {
	--bg-primario: #EF9F9F;
	--bg-secundario: #F47C7C;
	--titulo: #4a4545;
	--texto: #FFF2F2;
}

main.dark-mode {
	--bg-primario: #774360;
	--bg-secundario: #4C3A51;
	--titulo: #E7AB79;
	--texto: #FFF2F2;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
