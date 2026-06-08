<template>
  <div class="custom-select" ref="selectRef">
    <div class="select-trigger" :class="{ 'is-open': isOpen }" @click="isOpen = !isOpen">
      <span>{{ selectedLabel }}</span>
      <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <polyline points="6 9 12 15 18 9"></polyline>
      </svg>
    </div>
    <transition name="dropdown">
      <ul v-if="isOpen" class="select-options">
        <li 
          v-for="opt in options" 
          :key="opt.value" 
          :class="['select-option', { 'is-selected': modelValue === opt.value }]"
          @click="selectOption(opt.value)"
        >
          {{ opt.label }}
          <svg v-if="modelValue === opt.value" viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="20 6 9 17 4 12"></polyline>
          </svg>
        </li>
      </ul>
    </transition>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  modelValue: String,
  options: Array
})
const emit = defineEmits(['update:modelValue'])

const isOpen = ref(false)
const selectRef = ref(null)

const selectedLabel = computed(() => {
  const opt = props.options.find(o => o.value === props.modelValue)
  return opt ? opt.label : ''
})

function selectOption(val) {
  emit('update:modelValue', val)
  isOpen.value = false
}

function handleClickOutside(e) {
  if (selectRef.value && !selectRef.value.contains(e.target)) {
    isOpen.value = false
  }
}

onMounted(() => document.addEventListener('click', handleClickOutside))
onUnmounted(() => document.removeEventListener('click', handleClickOutside))
</script>

<style scoped>
.custom-select {
  position: relative;
  width: 100%;
}
.select-trigger {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 14px;
  background: #fff;
  border: 1px solid var(--c-border);
  border-radius: var(--radius-md);
  font-size: 13.5px;
  font-weight: 500;
  color: var(--c-text);
  cursor: pointer;
  transition: all .2s;
}
.select-trigger:hover {
  border-color: var(--c-border-strong);
}
.select-trigger.is-open {
  border-color: #74e5d6;
  box-shadow: 0 0 0 3px rgba(116, 229, 214, 0.15);
}
.select-trigger svg {
  color: var(--c-text-mute);
  transition: transform .2s;
}
.select-trigger.is-open svg {
  transform: rotate(180deg);
}

.select-options {
  position: absolute;
  top: calc(100% + 6px);
  left: 0;
  width: 100%;
  max-height: 280px;
  overflow-y: auto;
  background: #fff;
  border: 1px solid var(--c-border);
  border-radius: var(--radius-md);
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
  padding: 6px 0;
  margin: 0;
  list-style: none;
  z-index: 100;
}
.select-option {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 14px;
  font-size: 13.5px;
  color: var(--c-text-soft);
  cursor: pointer;
  transition: background .2s, color .2s;
}
.select-option:hover {
  background: var(--c-bg-soft);
  color: var(--c-text);
}
.select-option.is-selected {
  background: #f3fdfa;
  color: var(--c-teal-600);
  font-weight: 600;
}

/* Scrollbar styling for options */
.select-options::-webkit-scrollbar {
  width: 6px;
}
.select-options::-webkit-scrollbar-track {
  background: transparent;
}
.select-options::-webkit-scrollbar-thumb {
  background: var(--c-border-strong);
  border-radius: 10px;
}

/* Animations */
.dropdown-enter-active, .dropdown-leave-active {
  transition: opacity .2s, transform .2s;
  transform-origin: top;
}
.dropdown-enter-from, .dropdown-leave-to {
  opacity: 0;
  transform: translateY(-8px) scaleY(0.95);
}
</style>
