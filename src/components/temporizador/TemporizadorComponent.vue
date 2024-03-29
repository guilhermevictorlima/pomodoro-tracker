<template>
  <div class="tracker">
    <figure class="tracker__view">
      <img src="@/assets/tomato-icon.svg" alt="">
      <figcaption>
        <FormatadorTempoComponent :tempoEmSegundos="isTemporizadorRodando ? tempoEmSegundos : obterTempoRestanteEmSegundos()"/>
      </figcaption>
    </figure>

    <div class="tracker__view__work-cicles">
      Work Cycles Done: <strong>{{ contadorCiclosTrabalho }}</strong>
    </div>

    <div class="tracker__controls">
      <div class="tracker__controls--temporizador-inativo" v-if="!isTemporizadorAtivo">
        <button @click="reiniciarCiclosDeTrabalho()"
                class="btn btn-outline-dark tracker__controls__button"
                v-if="contadorCiclosTrabalho > 0"
                :disabled="this.$props.disabledButtons"
        >
          <font-awesome-icon icon="fa-solid fa-repeat"/>
        </button>
        <button @click="isTemporizadorAtivo = true"
                class="btn btn-outline-dark tracker__controls__button"
                :disabled="this.$props.disabledButtons"
        >
          <font-awesome-icon icon="fa-solid fa-forward-step"/>
        </button>
      </div>

      <div class="tracker__controls--temporizador-ativo" v-if="isTemporizadorAtivo">
        <button @click="pausarOuRetomarTemporizador()"
                class="btn btn-outline-dark tracker__controls__button"
                :disabled="this.$props.disabledButtons"
        >
          <font-awesome-icon :icon="`fa-solid ${iconePauseOuContinue}`"/>
        </button>
        <button @click="inativarTemporizador()"
                class="btn btn-outline-dark tracker__controls__button"
                :disabled="this.$props.disabledButtons"
        >
          <font-awesome-icon icon="fa-solid fa-stop"/>
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import FormatadorTempoComponent from '../formatadortempo/FormatadorTempoComponent.vue';

import notification from '@/assets/notification.mp3';

export default defineComponent({
	name: 'TemporizadorComponent',

	components: {
		FormatadorTempoComponent,
	},

	emits: ['temporizadorAtivo', 'cicloDeTrabalhoIniciado'],

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
		},
    disabledButtons: {
      type: Boolean,
      default: false
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
      this.$emit('temporizadorAtivo', isAtivo);
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

      this.$emit('cicloDeTrabalhoIniciado', this.isCicloDeTrabalho);

      if (this.isCicloDeTrabalho && this.tempoEmSegundos === 0) {
        this.contadorCiclosTrabalho += 1;
      }

			this.tempoEmSegundos = this.obterTempoRestanteEmSegundos();

			this.intervalId = setInterval(()=> {
				if (this.tempoEmSegundos === 0) {
          this.tocarAudio();
          this.inativarTemporizador();
				} else {
					this.tempoEmSegundos--;
				}
			}, 1000);
		},

    obterTempoRestanteEmSegundos(): number {

      let tempoTemporizador = this.isCicloDeTrabalho
          ? (this.$props.workTime || 1500)
          : (this.$props.shortBreakTime || 300);

      if (!this.isCicloDeTrabalho && this.isPausaLonga()) {
        tempoTemporizador = (this.$props.longBreakTime || 900);
      }

      return this.tempoEmSegundos !== 0 ? this.tempoEmSegundos : tempoTemporizador;
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
    },

    reiniciarCiclosDeTrabalho(): void {
      this.isCicloDeTrabalho = true;
      this.contadorCiclosTrabalho = 0;
    },

    tocarAudio(): void {
      new Audio(notification).play();
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

.tracker__controls__button:disabled {
  background-color: #c4c4c4;
}

</style>