<template>
  <fieldset class="form-group">
    <div class="form-group__container mb-3">
      <label v-if="$props.labelText" class="form-group__container__label">
        {{ labelText }}
      </label>
      <input
          ref="testtt"
          v-model="inputValue"
          class="form-group__container__input"
          :class="isInputInvalido ? 'form-group__container__input--invalid' : ''"
          maxlength="6"
          :placeholder="placeholder"
          @input="emitirValorInput()"
      />
      <span class="form-group__container__error-message" v-if="isInputInvalido">{{ mensagemDeErro }}</span>
    </div>
  </fieldset>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'InputTempoComponent',

  emits: ['digitar'],

  props: {
    labelText: {
      type: String
    },
    placeholder: {
      type: String
    },
  },

  data() {
    return  {
      inputValue: '',
      isInputInvalido: false
    }
  },

  methods: {
    emitirValorInput(): void {
      this.validar();

      if (!this.isInputInvalido) {
        this.$emit('digitar', this.inputValue);
      }
    },

    validar(): void {
      if (this.inputValue != '') {
        let intValue = parseInt(this.inputValue);
        this.isInputInvalido = isNaN(intValue) || intValue <= 0;
      } else {
        this.isInputInvalido = false;
      }
    }
  },

  computed: {
    mensagemDeErro(): string {
      return isNaN(parseInt(this.inputValue)) ? 'Enter time in minutes' : 'Enter a valid value in minutes';
    }
  }

})
</script>

<style>

@keyframes shake {
  10%,
  90% {
    transform: translate3d(-3px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(4px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-8px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(8px, 0, 0);
  }
}

.form-group__container {
  display: flex;
  flex-direction: column;

  width: 4rem;
}

.form-group__container__label {
  color: var(--cor-texto-input);
}

.form-group__container__input {
  width: 100%;
  outline: none;
  background: none;
  border: none;
  border-bottom: 1px solid var(--cor-borda-input);
  text-align: center;

  margin-top: 0.5rem;

  color: var(--cor-texto-input);
}

.form-group__container__input::placeholder {
  color: var(--cor-placeholder-input);
}

.form-group__container__input:focus::placeholder {
  color: transparent;
}

.form-group__container__input:disabled:hover {
  cursor: not-allowed;
}

.form-group__container__input--invalid {
  animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
}

.form-group__container__error-message {
  font-size: 0.8rem;
  color: #ff0000;
}


</style>