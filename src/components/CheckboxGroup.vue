<script setup>
const props = defineProps({
  checkboxes: Array,
  name: String,
});

const model = defineModel();
</script>

<template>
  <label v-for="checkbox in checkboxes">
    <input
      type="checkbox"
      :name="name"
      :value="checkbox.value"
      v-model="model"
    />
    <div class="labels">
      <div class="left">
        <div class="label--main">{{ checkbox.label }}</div>
        <div class="label-details">{{ checkbox.details }}</div>
      </div>
      <div class="right label--sublabel">{{ checkbox.sublabel }}</div>
    </div>
  </label>
</template>

<style scoped>
label {
  padding: 16px;
  border: solid 1px var(--light-gray);
  border-radius: 5px;
  cursor: pointer;
  display: flex;
  gap: 16px;
  align-items: center;

  &.selected {
    background-color: var(--alabaster);
    border-color: var(--purplish-blue);
  }

  &:hover {
    border-color: var(--purplish-blue);
  }
}

input {
  appearance: none;
  height: 21px;
  width: 21px;
  display: block;
  cursor: pointer;
  border-radius: 5px;
  border: solid 1px var(--light-gray);

  &:checked {
    background-color: var(--purplish-blue);
    background-image: url('/images/icon-checkmark.svg');
    background-repeat: no-repeat;
    background-position: center;
  }
}

.labels {
  flex: 1;
  display: flex;
  justify-content: space-between;
  align-items: center;

  & > .left {
    display: flex;
    flex-direction: column;
    gap: 2px;
  }
}

.label--main {
  font-weight: 500;
  color: var(--marine-blue);
}

.label--sublabel {
  color: var(--purplish-blue);
  font-weight: 500;
}

.label--details {
  color: var(--cool-gray);
}
</style>
