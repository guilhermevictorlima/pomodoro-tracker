<template>
	<div>
		<FormatadorTempoComponent :tempoEmSegundos="tempoEmSegundos"/>
	</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import FormatadorTempoComponent from '../formatador-tempo/FormatadorTempoComponent.vue';

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
		}
	},

	watch: {
		isTemporizadorRodando: function(isRodando) {
			if(isRodando) {
				this.iniciarContagem();
			} else {
				this.pararContagem();
			}
		}
	},

	methods: {

		iniciarContagem(): void {
			this.tempoEmSegundos = this.$props.tempoDoCiclo;

			this.intervalId = setInterval(()=> {
				if(this.tempoEmSegundos === 0) {
					this.$emit('aoFinalizarContagem');
				} else {
					this.tempoEmSegundos--;
				}				
			}, 1000);
		},

		pararContagem(): void {
			clearInterval(this.intervalId);
		},

	},

});
</script>

<style>

</style>