<script setup>
import { computed } from 'vue';
const props = defineProps({
  label: String,
  value: String,
  sublabel: String,
  details: String,
  name: String,
});

const model = defineModel();

/** @param {KeyboardEvent} event */
function handleKeyDown(event) {
  if (event.key === ' ') {
    model.value = props.value;
  }
}

const selected = computed(() => model.value === props.value);
</script>

<template>
  <div
    role="radio"
    :aria-checked="selected"
    tabindex="0"
    @keydown="handleKeyDown"
  >
    <label :class="{ selected }">
      <div class="container">
        <slot name="icon"></slot>
        <div class="labels">
          <div class="label--main">{{ label }}</div>
          <div class="label--sublabel">{{ sublabel }}</div>
          <Transition>
            <div v-if="details" class="label--details">
              {{ details }}
            </div>
          </Transition>
          <input type="radio" :value="value" v-model="model" />
        </div>
      </div>
    </label>
  </div>
</template>

<style scoped>
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

label {
  border: solid 1px var(--light-gray);
  border-radius: 5px;
  cursor: pointer;
  display: block;

  &.selected {
    background-color: var(--alabaster);
    border-color: var(--purplish-blue);
  }

  &:hover {
    border-color: var(--purplish-blue);
  }
}

.container {
  padding: 16px;
  display: flex;
  gap: 16px;
}

.labels {
  display: flex;
  flex-direction: column;
  gap: 3px;
}

.label--main {
  font-weight: 500;
  color: var(--marine-blue);
}

.label--sublabel {
  font-size: 14px;
  color: var(--cool-gray);
}

.label--details {
  font-size: 14px;
  color: var(--marine-blue);
}

input {
  display: none;
}
</style>
