<script setup>
import { inject } from 'vue';
import { billingCycleAbrevs } from '../copyMaps';
import FormWrapper from './FormWrapper.vue';
import Button from './Button.vue';
import CheckboxGroup from './CheckboxGroup.vue';

const props = defineProps({
  selectedBillingCycle: String,
});

const addOns = inject('addOns');
const actions = inject('actions');
const addOnsModel = defineModel('addOns');

/** @param {any} billingCycles */
function generatePrice(billingCycles) {
  return `+$${billingCycles[props.selectedBillingCycle].price}/${
    billingCycleAbrevs[props.selectedBillingCycle]
  }`;
}
</script>

<template>
  <FormWrapper
    title="Pick Add-ons"
    subtitle="Add-ons help enhance your gaming experience."
  >
    <template #form-content>
      <div>
        <fieldset>
          <CheckboxGroup
            name="add-ons"
            :checkboxes="
              addOns.map(
                ({ billingCycles, description, displayValue, value }) => ({
                  value,
                  details: description,
                  label: displayValue,
                  sublabel: generatePrice(billingCycles),
                })
              )
            "
            v-model="addOnsModel"
          ></CheckboxGroup>
        </fieldset>
      </div>
    </template>
    <template #form-actions>
      <Button
        type="submit"
        variant="secondary"
        @click.prevent="actions.goToNextStep"
        >Next Step</Button
      >
      <Button variant="tertiary" @click="actions.goToPrevStep">Go Back</Button>
    </template>
  </FormWrapper>
</template>

<style scoped>
fieldset {
  border: none;
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-bottom: 24px;
}
</style>
