<script setup>
import { ref, provide, computed, watch } from 'vue';
import PersonalInfo from './components/PersonalInfo.vue';
import Plan from './components/Plan.vue';
import AddOns from './components/AddOns.vue';
import Summary from './components/Summary.vue';
import Stepper from './components/Stepper.vue';
import plans from './mockdata/plans.json';
import addOnsData from './mockdata/addOns.json';

const name = ref('');
const email = ref('');
const phone = ref('');
const plan = ref(plans[0].value);
const addOns = ref([]);
const isYearly = ref(false);
const selectedPlan = computed(() => {
  return plans.find((p) => p.value === plan.value);
});
const selectedAddOns = computed(() =>
  addOnsData.filter((addOn) => addOns.value.includes(addOn.value))
);

const selectedBillingCycle = computed(() => {
  return isYearly.value ? 'yearly' : 'monthly';
});

const planPrice = computed(
  () => selectedPlan.value.billingCycles[selectedBillingCycle.value].price || 0
);
const addonsTotal = computed(() =>
  selectedAddOns.value.reduce((prev, current) => {
    return prev + current.billingCycles[selectedBillingCycle.value].price || 0;
  }, 0)
);

const total = computed(() => {
  return planPrice.value + addonsTotal.value;
});

const step = ref(4);
const goToNextStep = () => {
  step.value++;
};
const goToPrevStep = () => {
  step.value--;
};
const setStep = (s) => {
  step.value = s;
};
const submit = () => {
  console.log({
    name: name.value,
    email: email.value,
    phone: phone.value,
    plan: plan.value,
    addOns: Array.from(addOns.value),
    selectedBillingCycle: selectedBillingCycle.value,
  });
};

provide('actions', { goToNextStep, goToPrevStep, setStep, submit });
provide('plans', plans);
provide('addOns', addOnsData);
</script>

<template>
  <div class="flex-center-containter-vert">
    <div class="container">
      <Stepper :step="step"></Stepper>
      <div class="content-flex-wrapper">
        <div class="content-container">
          <PersonalInfo
            v-if="step === 1"
            v-model:name="name"
            v-model:email="email"
            v-model:phone="phone"
          ></PersonalInfo>
          <Plan
            v-if="step === 2"
            v-model:plan="plan"
            v-model:isYearly="isYearly"
            :selectedBillingCycle="selectedBillingCycle"
          ></Plan>
          <AddOns
            v-if="step === 3"
            v-model:addOns="addOns"
            :selectedBillingCycle="selectedBillingCycle"
          ></AddOns>
          <Summary
            v-if="step === 4"
            :plan="selectedPlan"
            :addOns="selectedAddOns"
            :selectedBillingCycle="selectedBillingCycle"
            :total="total"
          ></Summary>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.content-container {
  top: -70px;
  position: relative;
  width: 90%;
  height: 100%;
  margin: auto;
  background-color: white;
  border-radius: 10px;
  padding: 32px 24px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 34px;
}

@media (min-width: 800px) {
  .flex-center-containter-vert {
    display: flex;
    align-items: center;
    height: 100vh;
  }
  .container {
    flex: 1;
    display: flex;
    padding: 16px;
    align-items: stretch;
    max-width: 1100px;
    margin: auto;
  }

  .content-flex-wrapper {
    flex: 1;
  }

  .content-container {
    position: static;
    max-width: 600px;
    margin: auto;
    padding-right: 0;
    padding-left: 0;
    padding-bottom: 0;
  }
}
</style>
