<script setup>
defineProps({
  modelValue: {
    type: String,
    default: '',
  },
})
const emit = defineEmits(['update:modelValue'])
</script>
<template>
  <div class="search-bar">
    <input
      id="myInput"
      type="text"
      :value="modelValue"
      placeholder="Search by employee name, role, or certification..."
      @input="emit('update:modelValue', $event.target.value)"
    >
  </div>
</template>
<style scoped></style>