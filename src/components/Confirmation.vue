<script setup>
import { computed, inject } from 'vue';
import { billingCycleFull } from '../copyMaps';
import ThankYouIcon from './icons/ThankYouIcon.vue';

const props = defineProps({
  submissionData: Object,
});

const {
  name,
  email,
  phone,
  plan,
  billingCycle,
  addOns: choosenAddOns,
} = props.submissionData || {};

const plans = inject('plans');
const addOns = inject('addOns');

const selectedPlan = computed(() =>
  plans.find((planData) => planData.value === plan)
);

const selectedAddons = computed(() =>
  addOns.filter((addOn) => choosenAddOns?.includes(addOn.value))
);
</script>

<template>
  <ThankYouIcon></ThankYouIcon>
  <h2>Thank you!</h2>
  <p style="margin-bottom: 16px">
    Thanks for confirming your subscription! We hope you have fun using our
    platform. If you ever need support, please feel free to email us at
    support@loremgaming.com.
  </p>
  <p>Subscription summary:</p>
  <ul>
    <li>Name: {{ name }}</li>
    <li>Email: {{ email }}</li>
    <li>Phone: {{ phone }}</li>
    <li v-if="billingCycle">
      Plan: {{ selectedPlan.displayValue }} ({{
        billingCycleFull[billingCycle]
      }})
      <ul>
        <li v-for="addOn in selectedAddons">{{ addOn.displayValue }}</li>
      </ul>
    </li>
  </ul>
  <br />
  <div class="attribution">
    Challenge by
    <a href="https://www.frontendmentor.io?ref=challenge" target="_blank"
      >Frontend Mentor</a
    >. Coded by <a href="https://www.jimmysweeney.page">Jimmy Sweeney</a>.
  </div>
</template>

<style scoped>
.container {
  max-width: 400px;
  margin: auto;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: var(--cool-gray);
}

h2 {
  color: var(--marine-blue);
  margin-bottom: 16px;
  margin-top: 24px;
}

ul {
  list-style: none;
}
</style>
