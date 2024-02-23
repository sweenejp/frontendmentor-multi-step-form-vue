<script setup>
import { inject } from 'vue';
import { billingCycleAbrevs } from '../copyMaps';
import ArcadeIcon from './icons/ArcadeIcon.vue';
import AdvancedIcon from './icons/AdvancedIcon.vue';
import ProIcon from './icons/ProIcon.vue';
import FormWrapper from './FormWrapper.vue';
import Toggle from './Toggle.vue';
import Paper from './Paper.vue';
import Button from './Button.vue';
import RadioButton from './RadioButton.vue';

const props = defineProps({
  selectedBillingCycle: String,
});

const plans = inject('plans');
const actions = inject('actions');
const planModel = defineModel('plan');
const isYearly = defineModel('isYearly');
</script>

<template>
  <FormWrapper
    title="Select Your Plan"
    subtitle="You have the option of monthly or yearly billing."
  >
    <template #form-content>
      <div>
        <fieldset>
          <!-- NOTE The `v-for` , `v-if` directives feel totally wrong. Having those written in the same space as attributes/props is very confusing -->
          <RadioButton
            v-for="plan in plans"
            :key="plan.value"
            :value="plan.value"
            v-model="planModel"
            :label="plan.displayValue"
            :sublabel="`$${plan.billingCycles[selectedBillingCycle]?.price}/${billingCycleAbrevs[selectedBillingCycle]}`"
            :details="
              plan.billingCycles[selectedBillingCycle]?.monthsFree
                ? `${plan.billingCycles[selectedBillingCycle].monthsFree} months free`
                : ''
            "
          >
            <template #icon>
              <ArcadeIcon v-if="plan.value === 'arcade'" />
              <AdvancedIcon v-if="plan.value === 'advanced'" />
              <ProIcon v-if="plan.value === 'pro'" />
            </template>
          </RadioButton>
        </fieldset>
        <Paper
          :style="{
            display: 'flex',
            flexDirection: 'column',
            alignItems: 'center',
          }"
        >
          <Toggle
            name="is-yearly"
            onLabel="Yearly"
            offLabel="Monthly"
            v-model="isYearly"
          />
        </Paper>
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
