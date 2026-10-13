<script setup>
import { ref, useId, watch } from 'vue'
import DepartmentMember from './DepartmentMember.vue'
import StateToggle from './StateToggle.vue'

const props = defineProps({
  department: {
    type: String,
    required: true,
  },
  employees: {
    type: Array,
    required: true,
    },
})

const isActive = ref(false)
const panelId = `department-panel-${useId()}`
const departmentState = ref(0)
const memberStates = ref(props.employees.map(() => 0))

watch(
  () => props.employees,
  (employees) => {
    memberStates.value = employees.map(() => 0)
  },
)

function toggleAccordion() {
  isActive.value = !isActive.value
}

function setDepartmentState(state) {
  departmentState.value = state
  memberStates.value = props.employees.map(() => state)
}
</script>

<template>
  <section class="department-accordion">
    <div class="accordion-header">
      <button
        class="accordion"
        :class="{ active: isActive }"
        type="button"
        :aria-expanded="isActive"
        :aria-controls="panelId"
        @click="toggleAccordion"
      >
        {{ department }}
      </button>
      <StateToggle
        :model-value="departmentState"
        :name="department"
        show-labels
        @update:model-value="setDepartmentState"
      />
    </div>
    <Transition name="panel">
      <div :id="panelId" class="panel-wrapper" v-show="isActive">
        <div class="panel-content">
          <ul class="panel">
            <DepartmentMember
              v-for="(employee, index) in employees"
              :key="index"
              :employee="employee"
              :state-index="memberStates[index]"
              @update:state-index="memberStates[index] = $event"
            />
          </ul>
        </div>
      </div>
    </Transition>
  </section>
</template>

<style scoped>
.department-accordion{
  display: flex;
  flex-direction: column;
  width: 100%;
  border-radius: 5px;

}
.accordion-header{
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  padding: 0.5rem 15px;
  background-color: #eee;
}
.accordion{
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
.accordion::before{
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
.accordion.active::before{
    transform: rotate(45deg);
}
ul{
    list-style-type: none;
}
.panel{
    background-color: white;
    margin:0;
}
.panel{
  padding-inline-start: 0;
}
.panel-wrapper{
  display: grid;
  grid-template-rows: 1fr;
  transition: grid-template-rows 0.3s ease;
}
.panel-content{
  min-height: 0;
  overflow: hidden;
}
.panel-enter-from,
.panel-leave-to{
  grid-template-rows: 0fr;
}
</style>
