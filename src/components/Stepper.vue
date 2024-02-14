<script setup>
const props = defineProps({
  step: Number,
});
</script>

<template>
  <!-- Quote: With scoped, the parent component's styles will not leak into child components. However, a child component's root node will be affected by both the parent's scoped CSS and the child's scoped CSS. This is by design so that the parent can style the child root element for layout purposes.
  https://vuejs.org/api/sfc-css-features.html#scoped-css 
  This is why I have to name this class `stepper-container` here, because the parent component has a `container` class as well as the child's root node. I really don't understand this "feature". Svelte does not do this. This seems very counter-intuitive. If you wanted to control the layout of the child component from the parent, you could just pass a prop.
  -->
  <div class="stepper-container">
    <div class="step">
      <div class="bullet" :class="{ active: props.step === 1 }">1</div>
      <div class="step-text">
        <div class="label">Step 1</div>
        <div class="title">Your Info</div>
      </div>
    </div>
    <div class="step">
      <div class="bullet" :class="{ active: props.step === 2 }">2</div>
      <div class="step-text">
        <div class="label">Step 2</div>
        <div class="title">Select Plan</div>
      </div>
    </div>
    <div class="step">
      <div class="bullet" :class="{ active: props.step === 3 }">3</div>
      <div class="step-text">
        <div class="label">Step 3</div>
        <div class="title">Add-ons</div>
      </div>
    </div>
    <div class="step">
      <div class="bullet" :class="{ active: props.step === 4 }">4</div>
      <div class="step-text">
        <div class="label">Step 4</div>
        <div class="title">Summary</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.stepper-container {
  padding: 32px;
  background: url('/images/bg-sidebar-mobile.svg');
  background-repeat: no-repeat;
  display: flex;
  gap: 24px;
  justify-content: center;
  align-items: start;
  height: 170px;
}

.step {
  display: flex;
  gap: 16px;
  align-items: center;
}

.bullet {
  border-radius: 50%;
  border: solid white 1px;
  color: white;
  padding: 16px;
  height: 16px;
  width: 16px;
  display: flex;
  align-items: center;
  justify-content: center;

  &.active {
    background-color: var(--light-blue);
    border-color: var(--light-blue);
    color: var(--marine-blue);
  }
}

.step-text {
  display: none;
}

@media (min-width: 375px) {
  .stepper-container {
    background-size: cover;
  }
}

@media (min-width: 800px) {
  .stepper-container {
    background-image: url('/images/bg-sidebar-desktop.svg');
    background-position: center;
    border-radius: 10px;
    height: auto;
    flex-direction: column;
    justify-content: start;
    min-width: 300px;
  }

  .step-text {
    display: block;
    text-transform: uppercase;
  }
  .step-text > .label {
    color: var(--cool-gray);
  }

  .step-text > .title {
    color: white;
    font-weight: 500;
    letter-spacing: 2px;
  }
}
</style>
