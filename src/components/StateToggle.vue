<script setup>
defineProps({
  modelValue: {
    type: Number,
    required: true,
  },
  name: {
    type: String,
    required: true,
  },
  showLabels: {
    type: Boolean,
    default: false,
  },
})

defineEmits(['update:modelValue'])

const stateLabels = ['None', 'View', 'Assigned']
</script>

<template>
  <div class="state-control" role="group" :aria-label="`${name} assignment state`">
    <div v-if="showLabels" class="state-labels" aria-hidden="true">
      <span v-for="label in stateLabels" :key="label">{{ label }}</span>
    </div>
    <div class="state-options-container"><div class="state-options">
      <div class="state-track" aria-hidden="true">
        <span class="state-thumb" :class="`state-${modelValue}`">
          <span v-if="modelValue === 0" class="cross-icon">✖</span>
          <svg v-else-if="modelValue === 1" class="state-icon" viewBox="0 0 24 24">
            <path d="M2 12s3.5-7 10-7 10 7 10 7-3.5 7-10 7S2 12 2 12Z" />
            <circle cx="12" cy="12" r="3" />
          </svg>
          <svg v-else class="state-icon" viewBox="0 0 24 24">
            <path d="M9 6h11M9 12h11M9 18h11" />
            <path d="m3 6 1.5 1.5L7 5M3 12l1.5 1.5L7 11M3 18l1.5 1.5L7 17" />
          </svg>
        </span>
      </div>
      <button
        v-for="(label, index) in stateLabels"
        :key="label"
        class="state-option"
        type="button"
        :aria-label="`${name}: ${label}`"
        :aria-pressed="modelValue === index"
        @click="$emit('update:modelValue', index)"
      />
    </div></div>
    
  </div>
</template>

<style scoped>
.state-control{
  flex: 0 0 8rem;
  grid-template-columns:1fr;

}
.state-labels,
.state-options{
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
}
.state-labels{
  margin-bottom: 0.25rem;
  font-size: 0.75rem;
  text-align: center;
}
.state-options{
  position: relative;
  width: 5.4rem;
  min-width: 0;
  height: 2rem;
}
.state-options-container{
  width:100%;
  display: flex;
  justify-content:center;
}
.state-track{
  position: absolute;
  inset: 0;
  border: 1px solid #979494b0;
  padding:.25rem;
  height:100%;
  border-radius: 999px;
  background: #ffd3d3;
}
.state-option{
  z-index: 1;
  width: 100%;
  min-width: 0;
  height: 100%;
  padding: 0;
  border: 0;
  border-radius: 999px;
  background: transparent;
  color: #222;
  cursor: pointer;
}
.state-option:focus-visible{
  outline: 2px solid #2563eb;
  outline-offset: -2px;
}
.state-thumb{
  position: absolute;
  inset-block: 3px;
  display: grid;
  width: calc(33.33333% - 2px);
  place-items: center;
  border-radius: 999px;
  background-color: rgb(238, 155, 155);
  box-shadow: 0 1px 3px #0003;
  color: #222;
  transition: transform 0.2s ease,background-color 0.2s ease;
}
.state-thumb.state-1{
  transform: translateX(100%);
  background: rgb(232, 124, 124);
}
.state-thumb.state-2{
  transform: translateX(calc(200% - 2px));
  background-color: rgb(241, 52, 52);
}
.cross-icon{
  font-size: .9rem;
  line-height: 1.05;
}
.state-icon{
  width: .9rem;
  height: .9rem;
  fill: none;
  stroke: currentColor;
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-width: 2;
}
</style>
