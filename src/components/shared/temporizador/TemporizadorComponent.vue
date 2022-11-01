<template>
  <div class="tracker">
    <figure class="tracker__view">
      <img src="@/assets/tomato-icon.svg" alt="">
      <figcaption>
        <FormatadorTempoComponent :tempoEmSegundos="tempoEmSegundos"/>
      </figcaption>
    </figure>

    <div class="tracker__controls">
      <div class="tracker__controls--temporizador-inativo" v-if="!temporizadorAtivo">
        <button @click="temporizadorAtivo = true" class="btn btn-outline-dark tracker__controls__button">
          <font-awesome-icon icon="fa-solid fa-forward-step"/>
        </button>
      </div>

      <div class="tracker__controls--temporizador-ativo" v-if="temporizadorAtivo">
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

	emits: ['aoFinalizarContagem'],

	props: {
		tempoDoCiclo: {
			required: true,
			type: Number
		}
	},

	data() {
		return {
			tempoEmSegundos: 0,
			intervalId: 0,
      temporizadorAtivo: false,
      isTemporizadorRodando: false
		}
	},

	watch: {
    temporizadorAtivo: function (isAtivo: boolean)  {
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
			this.tempoEmSegundos = this.obterTempoRestanteEmSegundos();

			this.intervalId = setInterval(()=> {
				if (this.tempoEmSegundos === 0) {
					this.$emit('aoFinalizarContagem');
				} else {
					this.tempoEmSegundos--;
				}
			}, 1000);
		},

    obterTempoRestanteEmSegundos(): number {
      return this.tempoEmSegundos !== 0 && this.tempoEmSegundos != this.$props.tempoDoCiclo ? this.tempoEmSegundos : this.$props.tempoDoCiclo;
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
      this.temporizadorAtivo = false;
      this.$emit('aoFinalizarContagem');
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

.tracker__view img {
  width: 18rem;
}

.tracker__view figcaption {
  position: relative;
  bottom: 8rem;
  color: white;
  font-size: 2rem;
}

.tracker__controls__button {
  border-radius: 100%;
  background-color: white;
}

.tracker__controls__button:last-child {
  margin-left: 0.5rem;
}

</style>