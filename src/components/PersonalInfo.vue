<script setup>
import { inject, ref } from 'vue';
import FormWrapper from './FormWrapper.vue';
import TextField from './TextField.vue';
import Button from './Button.vue';
import hasError from '../hasError';

const name = defineModel('name');
const email = defineModel('email');
const phone = defineModel('phone');

const actions = inject('actions');

const errors = ref({ name: '', email: '', phone: '' });

/** @param {Event} event */
function handleNext(event) {
  event.preventDefault();
  const formEl = document.querySelector('form');
  /** @type {HTMLInputElement} */
  let erroredInput;
  formEl?.querySelectorAll('input').forEach((element) => {
    const { name } = element;
    const error = hasError(element);
    errors.value[name] = error;
    if (error && !erroredInput) {
      erroredInput = element;
    }
  });

  if (erroredInput) {
    erroredInput.focus();

    return;
  }

  actions.goToNextStep();
}
</script>

<template>
  <FormWrapper
    title="Personal info"
    subtitle="Please provide your name, email address, and phone number."
  >
    <template #form-content>
      <div class="form-content">
        <TextField
          label="Name"
          type="text"
          name="name"
          placeholder="e.g Stephen King"
          required
          v-model="name"
          v-model:error="errors.name"
        />
        <TextField
          label="Email Address"
          type="email"
          name="email"
          placeholder="e.g stephenking@lorem.com"
          required
          v-model="email"
          v-model:error="errors.email"
        />
        <TextField
          label="Phone Number"
          type="tel"
          name="phone"
          placeholder="e.g +1 234 567 890"
          required
          v-model="phone"
          v-model:error="errors.phone"
        />
      </div>
    </template>
    <template #form-actions>
      <Button variant="secondary" @click.prevent="handleNext">Next Step</Button>
    </template>
  </FormWrapper>
</template>

<style scoped>
.form-content {
  display: flex;
  flex-direction: column;
  gap: 16px;
}
</style>
