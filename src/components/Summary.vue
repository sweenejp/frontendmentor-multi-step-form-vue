<script setup>
import { inject } from 'vue';
import {
  billingCycleAbrevs,
  billingCycleFull,
  billingCyclePer,
} from '../copyMaps';
import Button from './Button.vue';
import FormWrapper from './FormWrapper.vue';
import Paper from './Paper.vue';

defineProps({
  plan: Object,
  addOns: Array,
  total: Number,
  selectedBillingCycle: String,
});

const actions = inject('actions');
</script>

<template>
  <FormWrapper
    title="Finishing up"
    subtitle="Double-check everything looks OK before confirming."
  >
    <template #form-content
      ><div>
        <Paper :style="{ padding: '24px' }">
          <div class="plan" :class="{ 'has-add-ons': addOns?.length }">
            <p class="plan-value">
              {{ plan?.displayValue }} ({{
                billingCycleFull[selectedBillingCycle]
              }})
            </p>
            <div class="plan-bottom">
              <button class="change-plan" @click.prevent="actions.setStep(2)">
                Change
              </button>
              <p class="plan-price">
                ${{ plan?.billingCycles[selectedBillingCycle]?.price }}/{{
                  billingCycleAbrevs[selectedBillingCycle]
                }}
              </p>
            </div>
          </div>
          <div class="add-ons">
            <div v-for="addOn in addOns" class="add-on">
              <p class="add-on-value">{{ addOn.displayValue }}</p>
              <p class="add-on-price">
                +${{ addOn.billingCycles[selectedBillingCycle].price }}/{{
                  billingCycleAbrevs[selectedBillingCycle]
                }}
              </p>
            </div>
          </div>
        </Paper>
        <div class="total">
          <p class="total-text">
            Total ({{ billingCyclePer[selectedBillingCycle] }})
          </p>
          <p class="total-price">
            ${{ total }}/{{ billingCycleAbrevs[selectedBillingCycle] }}
          </p>
        </div>
      </div>
    </template>
    <template #form-actions>
      <Button type="submit" variant="primary">Confirm</Button>
      <Button variant="tertiary" @click="actions.goToPrevStep">Go Back</Button>
    </template>
  </FormWrapper>
</template>

<style scoped>
.plan {
  padding-bottom: 24px;

  &.has-add-ons {
    border-bottom: 1px solid var(--light-gray);
  }
}

.plan-value {
  font-weight: 500;
  font-size: 18px;
}

.plan-bottom {
  display: flex;
  justify-content: space-between;
}

.change-plan {
  appearance: none;
  border: none;
  background-color: transparent;
  text-decoration: underline;
  color: var(--cool-gray);
  cursor: pointer;
}

.plan-price {
  font-weight: 500;
  color: var(--marine-blue);
}

.add-ons {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding-top: 16px;
}

.add-on {
  display: flex;
  justify-content: space-between;
  font-size: 14px;
}

.add-on-value {
  color: var(--cool-gray);
}
.add-on-price {
  color: var(--marine-blue);
}

.total {
  display: flex;
  justify-content: space-between;
  padding: 24px;
}

.total-text {
  color: var(--cool-gray);
  font-size: 14px;
}

.total-price {
  color: var(--purplish-blue);
  font-size: 20px;
  font-weight: 700;
}
</style>
