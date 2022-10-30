<template>
  <div>
    <figure>
      <img src="@/assets/tomato-icon.svg" alt="">
      <figcaption>
        <FormatadorTempoComponent :tempoEmSegundos="tempoEmSegundos"/>
      </figcaption>
    </figure>
    <div class="pomodoro-view__tracker-controls--temporizador-ativo" v-if="isTemporizadorRodando">

      <button @click="pausarOuRetomarTemporizador()" class="btn btn-outline-dark pomodoro-view__tracker-controls__botao">
        <font-awesome-icon :icon="`fa-solid ${iconePauseOuContinue}`"/>
      </button>
      <button @click="interromperTemporizador()" class="btn btn-outline-dark pomodoro-view__tracker-controls__botao">
        <font-awesome-icon icon="fa-solid fa-stop"/>
      </button>

    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import FormatadorTempoComponent from '../formatadortempo/FormatadorTempoComponent.vue';

export default defineComponent({
	name: 'TemporizadorComponent',

	components: {
		FormatadorTempoComponent,
	},

	emits: ['aoFinalizarContagem'],

	props: {
		tempoDoCiclo: {
			required: true,
			type: Number
		},

		isTemporizadorRodando: {
			required: true,
			type: Boolean
		}
	},

	data() {
		return {
			tempoEmSegundos: 0,
			intervalId: 0,
      temporizadorEstaRodando: false
		}
	},

	watch: {
		isTemporizadorRodando: function (isRodando: boolean) {
      this.temporizadorEstaRodando = isRodando;
		},

    temporizadorEstaRodando: function (isRodando: boolean) {
      if (isRodando) {
        this.iniciarContagem();
      } else {
        this.pararContagem();
      }
    }
	},

	methods: {

		iniciarContagem(): void {
			this.tempoEmSegundos = this.tempoRestanteEmSegundos();

      console.log(this.tempoEmSegundos);

			this.intervalId = setInterval(()=> {
				if (this.tempoEmSegundos === 0) {
					this.$emit('aoFinalizarContagem');
				} else {
					this.tempoEmSegundos--;
				}
			}, 1000);
		},

    tempoRestanteEmSegundos(): number {
      return this.tempoEmSegundos !== 0 && this.tempoEmSegundos != this.$props.tempoDoCiclo ? this.tempoEmSegundos : this.$props.tempoDoCiclo;
    },

		pararContagem(): void {
			clearInterval(this.intervalId);
		},

    pausarOuRetomarTemporizador(): void {
      this.temporizadorEstaRodando = !this.temporizadorEstaRodando;
    },

    interromperTemporizador(): void {
      this.pararContagem();
      this.tempoEmSegundos = 0;
      this.$emit('aoFinalizarContagem');
    }

	},

  computed: {

    iconePauseOuContinue(): string {
      return this.temporizadorEstaRodando ? 'fa-pause' : 'fa-play';
    }

  }

});
</script>

<style scoped>

img {
  width: 18rem;
}

figcaption {
  position: relative;
  bottom: 8rem;
  color: white;
  font-size: 2rem;
}

</style>