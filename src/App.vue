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

</style>
