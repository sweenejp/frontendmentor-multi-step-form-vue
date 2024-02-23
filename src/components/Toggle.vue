<script setup>
const props = defineProps({
  name: String,
  offLabel: String,
  onLabel: String,
});

const checked = defineModel();

/** @param {KeyboardEvent} event */
function handleKeyDown(event) {
  if (event.key === ' ') {
    checked.value = !checked.value;
  }
}
</script>

<template>
  <label>
    <span class="off-label"> {{ offLabel }} </span>
    <div
      class="outer"
      role="checkbox"
      :aria-checked="checked"
      tabindex="0"
      @keydown="handleKeyDown"
    >
      <!-- Vue's syntax is very odd. I don't understand why the dynamic class has to be written this way. If `checked` were a prop, then this wouldn't need the curly braces (see Button's `variant` class). But since it is a ref it does? -->
      <div class="inner" :class="{ checked }"></div>
      <input type="checkbox" v-model="checked" />
    </div>
    <span class="on-label"> {{ onLabel }} </span>
  </label>
</template>

<style scoped>
label {
  display: inline-flex;
  gap: 8px;
}

input {
  display: none;
}

.outer {
  display: block;
  width: 42px;
  height: 21px;
  background-color: var(--marine-blue);
  border-radius: 20px;
  cursor: pointer;
  position: relative;
}

.inner {
  content: '';
  width: 15px;
  height: 15px;
  background-color: white;
  border-radius: 20px;
  position: absolute;
  top: 3px;
  left: 4px;
  transition: transform ease-in-out 0.1s;

  &.checked {
    transform: translateX(19px);
  }
}
</style>
