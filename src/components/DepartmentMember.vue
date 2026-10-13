<script setup>
  import { ref } from "vue";
  import StateToggle from "./StateToggle.vue";

  const props = defineProps({
    employee: {
      type: Object,
      required: true,
    },
    stateIndex: {
      type: Number,
      required: true,
    },
  });
  const isActive = ref(false);
  function toggleAccordion() {
    isActive.value = !isActive.value;
  }
  defineEmits(["update:stateIndex"]);
</script>

<template>
  <li class="member-row">
    <div class="accordion-header">
      <button
        class="accordion"
        :class="{ active: isActive }"
        type="button"
        :aria-expanded="isActive"
        @click="toggleAccordion"
      >
      <div class="employee-info">
        <div class="employee-name">{{ props.employee.name }}</div>
        <div class="employee-role">{{ props.employee.role }}</div>
      </div>
      </button>
      <StateToggle
        :model-value="props.stateIndex"
        :name="props.employee.name"
        @update:model-value="$emit('update:stateIndex', $event)"
      />
    </div>
    <Transition name="panel">
      <div class="panel-wrapper" v-show="isActive">
        <div class="panel-content">
          <div class="certification-list">
            <div class="certification" v-for="cert in employee.certs">
              {{ cert }}
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </li>
</template>

<style scoped>
  .employee-info{
    line-height: 1.25em;
  }
  .member-row {
    display: flex;
    flex-direction: column;
    padding: 10px 15px;
    margin-left: 0;
    border-bottom: 1px solid grey;
    list-style: none;

  }
  .employee-role{
    color:rgb(147, 150, 153);
  }
  .panel-wrapper {
    display: grid;
    grid-template-rows: 1fr;
    transition: grid-template-rows 0.3s ease;
  }
  .panel-enter-from,
  .panel-leave-to {
    grid-template-rows: 0fr;
  }
  .panel-content {
    min-height: 0;
    overflow: hidden;
  }
  .accordion-header {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
  }
  .accordion {
    display: flex;
    align-items: center;
    flex: 1;
    justify-content: flex-start;
    background: transparent;
    color: black;
    cursor: pointer;
    padding: 10px 0;
    border: none;
    text-align: left;
    outline: none;
    transition: 0.4s;
  }
  .accordion::before {
    content: "";
    flex: 0 0 0.45rem;
    width: 0.45rem;
    height: 0.45rem;
    margin-right: 0.75rem;
    border-right: 2px solid currentColor;
    border-bottom: 2px solid currentColor;
    transform: rotate(-45deg);
    transition: transform 0.2s ease;
  }
  .accordion.active::before {
    transform: rotate(45deg);
  }
  .certification-list {
    padding-top:1rem;
    display: flex;
    flex-direction: row;
  }
  .certification-list > *:not(:last-child) {
    margin-right: 5px;
  }
  .certification {
    border-radius: 15px;
    background: rgb(207, 202, 202);
    padding-inline: 10px;
    font-size: 90%;
    padding-block: 5px;
    width: fit-content;
  }
</style>
