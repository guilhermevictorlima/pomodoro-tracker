<template>
	{{ tempoFormatado }}
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
	name: 'FormatadorTempoComponent',

	props: {
		tempoEmSegundos: {
			required: true,
			type: Number
		}
	},

	methods: {
		obterHoraMinutoSegundo(tempoEmDate: Date): string {
			const horaEmSegundos = 3600;
			const posicaoHoraOuMinuto = this.$props.tempoEmSegundos > horaEmSegundos ? 11 : 14;

			return tempoEmDate.toISOString().substring(posicaoHoraOuMinuto, 19);
		},
		
	},

	computed: {
		tempoFormatado(): string {
			return this.obterHoraMinutoSegundo(new Date(this.$props.tempoEmSegundos * 1000));
		}
	}
});
</script>