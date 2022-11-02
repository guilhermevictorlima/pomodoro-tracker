<template>
  <fieldset class="form-group">
    <div class="form-group__container mb-3">
      <label v-if="$props.labelText" class="form-group__container__label">
        {{ labelText }}
      </label>
      <input
          v-model="inputValue"
          class="form-group__container__input"
          maxlength="6"
          :placeholder="placeholder"
          @keyup="emitirValorInput()"
      />
    </div>
  </fieldset>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'InputTempoComponent',

  emits: ['aoDigitar'],

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
      inputValue: ''
    }
  },

  methods: {
    emitirValorInput(): void {
      this.$emit('aoDigitar', this.inputValue);
    }
  }

})
</script>

<style>

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


</style>