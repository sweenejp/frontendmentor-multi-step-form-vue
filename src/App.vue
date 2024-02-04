<script setup>
import { ref, provide } from 'vue';
import PersonalInfo from './components/PersonalInfo.vue';
import Plan from './components/Plan.vue';
import Stepper from './components/Stepper.vue';

const name = ref('');
const email = ref('');
const phone = ref('');

const step = ref(1);
const goToNextStep = () => {
  step.value++;
};

provide('actions', { goToNextStep });
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
          <Plan v-if="step === 2"></Plan>
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
