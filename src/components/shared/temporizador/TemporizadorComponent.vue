<template>
  <div class="tracker">
    <figure class="tracker__view">
      <img src="@/assets/tomato-icon.svg" alt="">
      <figcaption>
        <FormatadorTempoComponent :tempoEmSegundos="tempoEmSegundos"/>
      </figcaption>
    </figure>

    <div class="tracker__view__work-cicles">
      Work Cycles Done: <strong>{{ contadorCiclosTrabalho }}</strong>
    </div>

    <div class="tracker__controls">
      <div class="tracker__controls--temporizador-inativo" v-if="!isTemporizadorAtivo">
        <button @click="reiniciarCiclosDeTrabalho()"
                class="btn btn-outline-dark tracker__controls__button"
                v-if="contadorCiclosTrabalho > 0">
          <font-awesome-icon icon="fa-solid fa-repeat"/>
        </button>
        <button @click="isTemporizadorAtivo = true" class="btn btn-outline-dark tracker__controls__button">
          <font-awesome-icon icon="fa-solid fa-forward-step"/>
        </button>
      </div>

      <div class="tracker__controls--temporizador-ativo" v-if="isTemporizadorAtivo">
        <button @click="pausarOuRetomarTemporizador()" class="btn btn-outline-dark tracker__controls__button">
          <font-awesome-icon :icon="`fa-solid ${iconePauseOuContinue}`"/>
        </button>
        <button @click="inativarTemporizador()" class="btn btn-outline-dark tracker__controls__button">
          <font-awesome-icon icon="fa-solid fa-stop"/>
        </button>
      </div>
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

	emits: ['finalizarContagem'],

	props: {
		workTime: {
			required: true,
			type: Number
		},
		shortBreakTime: {
			required: true,
			type: Number
		},
    longBreakTime: {
			required: true,
			type: Number
		}
	},

	data() {
		return {
      contadorCiclosTrabalho: 0,
			tempoEmSegundos: 0,
			intervalId: 0,
      isTemporizadorAtivo: false,
      isTemporizadorRodando: false,
      isCicloDeTrabalho: true
		}
	},

	watch: {
    isTemporizadorAtivo: function (isAtivo: boolean)  {
      this.isTemporizadorRodando = isAtivo;
    },

    isTemporizadorRodando: function (isRodando: boolean) {
      if (isRodando) {
        this.iniciarContagem();
      } else {
        this.pararContagem();
      }
    }
	},

	methods: {
		iniciarContagem(): void {

      if (this.isCicloDeTrabalho) {
        this.contadorCiclosTrabalho += 1;
      }

			this.tempoEmSegundos = this.obterTempoRestanteEmSegundos();

			this.intervalId = setInterval(()=> {
				if (this.tempoEmSegundos === 0) {
					this.inativarTemporizador();
				} else {
					this.tempoEmSegundos--;
				}
			}, 1000);
		},

    obterTempoRestanteEmSegundos(): number {

      let tempoTemporizador = this.isCicloDeTrabalho ? this.$props.workTime : this.$props.shortBreakTime;

      if (!this.isCicloDeTrabalho && this.isPausaLonga()) {
        tempoTemporizador = this.$props.longBreakTime;
      }

      return this.tempoEmSegundos !== 0 && this.tempoEmSegundos != tempoTemporizador ? this.tempoEmSegundos : tempoTemporizador;
    },

		pararContagem(): void {
			clearInterval(this.intervalId);
		},

    pausarOuRetomarTemporizador(): void {
      this.isTemporizadorRodando = !this.isTemporizadorRodando;
    },

    inativarTemporizador(): void {
      this.pararContagem();
      this.tempoEmSegundos = 0;
      this.isTemporizadorAtivo = false;

      this.isCicloDeTrabalho = !this.isCicloDeTrabalho;
      this.$emit('finalizarContagem');
    },

    reiniciarCiclosDeTrabalho(): void {
      this.contadorCiclosTrabalho = 0;
    },

    isPausaLonga(): boolean {
      let numeroDeCiclosEMultiploDeQuatro = this.contadorCiclosTrabalho % 4 === 0;
      return numeroDeCiclosEMultiploDeQuatro;
    }
	},

  computed: {
    iconePauseOuContinue(): string {
      return this.isTemporizadorRodando ? 'fa-pause' : 'fa-play';
    }
  }

});
</script>

<style>

.tracker__view {
  margin-bottom: -1rem;
}

.tracker__view img {
  width: 18rem;
}

.tracker__view figcaption {
  position: relative;
  bottom: 8rem;
  color: white;
  font-size: 2rem;
}

.tracker__view__work-cicles {
  margin-bottom: 1rem;
  font-size: 1rem;
  color: #EDDBC0;
}

.tracker__controls__button {
  border-radius: 100%;
  background-color: white;
}

.tracker__controls__button:last-child {
  margin-left: 0.5rem;
}

</style>