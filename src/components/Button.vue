<script setup>
const props = defineProps({
  variant: String,
  disabled: Boolean,
  loading: Boolean,
});
</script>

<template>
  <button :class="variant" :disabled="Boolean(disabled || loading)">
    <span v-if="loading">loading...</span>
    <slot v-else></slot>
  </button>
</template>

<style>
button {
  padding: 10px 18px;
  font-weight: 500;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: opacity ease-in-out 0.1s;
}

.tertiary {
  background-color: transparent;
  color: var(--cool-gray);

  &:hover {
    opacity: 0.8;
  }

  &:focus {
    opacity: 0.8;
  }
}

.secondary {
  background-color: var(--marine-blue);
  color: white;

  &:hover {
    opacity: 0.8;
  }

  &:focus {
    opacity: 0.8;
  }
}

.primary {
  background-color: var(--purplish-blue);
  color: white;

  &:hover {
    opacity: 0.8;
  }

  &:focus {
    opacity: 0.8;
  }
}
</style>
