<script setup>
import hasError from '../hasError';
// NOTE: not a fan, this might be better with typescript, but this feels like a really ugly way to declare a component's props
const props = defineProps({
  label: String,
  value: String,
  type: String,
  name: String,
  placeholder: String,
  required: Boolean,
});
const model = defineModel();
// NOTE - two way binding for anything. In this instance, `error` is something that can be controlled from inside the component or outside. In other words, it's like internal state but also a prop. In React, you'd have to do ahieve this with a useEffect
const errorModel = defineModel('error');

/** @param {Event} event */
function handleInput(event) {
  if (!event.target || !(event.target instanceof HTMLInputElement)) {
    return;
  }

  // validate on input change only if there is an existing error
  if (!errorModel.value) {
    return;
  }

  errorModel.value = hasError(event.target);
}

/** @param {FocusEvent} event */
function handleBlur(event) {
  if (!event.target || !(event.target instanceof HTMLInputElement)) {
    return;
  }

  errorModel.value = hasError(event.target);
}
</script>

<template>
  <label>
    <div class="label-text">
      {{ label }}
      <strong v-if="Boolean(errorModel)" class="error-message">{{
        error
      }}</strong>
    </div>
    <input
      v-if="type === 'text'"
      type="text"
      :name="name"
      :placeholder="placeholder"
      :required="required"
      v-model="model"
      @input="handleInput"
      @blur="handleBlur"
    />
    <input
      v-if="type === 'email'"
      type="email"
      :name="name"
      :placeholder="placeholder"
      :required="required"
      v-model="model"
      @input="handleInput"
      @blur="handleBlur"
    />
    <input
      v-if="type === 'tel'"
      type="tel"
      :name="name"
      :placeholder="placeholder"
      :required="required"
      v-model="model"
      @input="handleInput"
      @blur="handleBlur"
    />
  </label>
</template>

<style scoped>
label {
  color: var(--marine-blue);
  font-size: 14px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.label-text {
  display: flex;
  justify-content: space-between;
}

.error-message {
  color: var(--strawberry-red);
  text-align: right;
}

input {
  color: var(--marine-blue);
  font-weight: 500;
  padding: 12px;
  display: block;
  border-radius: 5px;
  border: 1px solid var(--light-gray);
  outline: none;

  &:focus {
    border-color: var(--purplish-blue);
  }
}

.error {
  border-color: var(--strawberry-red);
}
</style>
