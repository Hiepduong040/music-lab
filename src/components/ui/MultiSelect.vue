<template>
  <div class="multi-select" ref="selectRef">
    <div class="select-trigger" :class="{ 'is-open': isOpen }" @click="openDropdown">
      <div class="tags">
        <span v-for="tag in selectedTags" :key="tag.value" class="tag">
          {{ tag.label }}
          <button class="remove-tag" @click.stop="removeTag(tag.value)" aria-label="Xoá">
            <svg viewBox="0 0 24 24" width="12" height="12" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>
          </button>
        </span>
        <input 
          ref="inputRef"
          v-model="search" 
          @keydown.enter.prevent="onEnter"
          @keydown.backspace="onBackspace"
          :placeholder="selectedTags.length === 0 ? 'Chọn hoặc gõ thẻ tag...' : ''" 
          class="select-input"
        />
      </div>
      <div class="chevron-wrapper">
        <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="chevron">
          <polyline points="6 9 12 15 18 9"></polyline>
        </svg>
      </div>
    </div>
    
    <transition name="dropdown">
      <ul v-if="isOpen" class="select-options">
        <li 
          v-for="opt in filteredOptions" 
          :key="opt.value" 
          :class="['select-option', { 'is-selected': isSelected(opt.value) }]"
          @click.stop="toggleOption(opt)"
        >
          <div class="checkbox">
            <svg v-if="isSelected(opt.value)" viewBox="0 0 24 24" width="12" height="12" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="20 6 9 17 4 12"></polyline>
            </svg>
          </div>
          {{ opt.label }}
        </li>
        <li v-if="filteredOptions.length === 0 && search" class="select-option select-create" @click.stop="onEnter">
          Tạo thẻ: <strong>{{ search }}</strong>
          <span>(Nhấn Enter)</span>
        </li>
      </ul>
    </transition>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  modelValue: { type: Array, default: () => [] },
  options: { type: Array, default: () => [] }
})
const emit = defineEmits(['update:modelValue'])

const isOpen = ref(false)
const search = ref('')
const selectRef = ref(null)
const inputRef = ref(null)

const selectedTags = computed(() => {
  return props.modelValue.map(val => {
    const opt = props.options.find(o => o.value === val)
    return opt ? opt : { value: val, label: val } // custom tag
  })
})

const filteredOptions = computed(() => {
  if (!search.value) return props.options
  const s = search.value.toLowerCase()
  return props.options.filter(o => o.label.toLowerCase().includes(s))
})

function isSelected(val) {
  return props.modelValue.includes(val)
}

function openDropdown() {
  isOpen.value = true
  inputRef.value?.focus()
}

function toggleOption(opt) {
  let newValue = [...props.modelValue]
  if (newValue.includes(opt.value)) {
    newValue = newValue.filter(v => v !== opt.value)
  } else {
    newValue.push(opt.value)
  }
  emit('update:modelValue', newValue)
  search.value = ''
  inputRef.value?.focus()
}

function removeTag(val) {
  emit('update:modelValue', props.modelValue.filter(v => v !== val))
}

function onEnter() {
  const s = search.value.trim()
  if (s) {
    const exact = props.options.find(o => o.label.toLowerCase() === s.toLowerCase())
    if (exact) {
      if (!isSelected(exact.value)) {
        emit('update:modelValue', [...props.modelValue, exact.value])
      }
    } else {
      if (!isSelected(s)) {
        emit('update:modelValue', [...props.modelValue, s])
      }
    }
    search.value = ''
  }
}

function onBackspace() {
  if (search.value === '' && props.modelValue.length > 0) {
    const newValue = [...props.modelValue]
    newValue.pop()
    emit('update:modelValue', newValue)
  }
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
.multi-select { position: relative; width: 100%; }
.select-trigger {
  display: flex; align-items: center; justify-content: space-between;
  min-height: 48px; padding: 6px 14px;
  background: #fff; border: 1px solid var(--c-border); border-radius: var(--radius-md);
  cursor: text; transition: all .2s;
}
.select-trigger:hover { border-color: var(--c-border-strong); }
.select-trigger.is-open { border-color: #74e5d6; box-shadow: 0 0 0 3px rgba(116, 229, 214, 0.15); }

.tags { display: flex; flex-wrap: wrap; gap: 8px; flex: 1; align-items: center; }
.tag {
  background: #f0fdfa; color: var(--c-teal-700); border: 1px solid #ccfbf1;
  font-size: 13.5px; font-weight: 600; padding: 4px 6px 4px 12px; border-radius: var(--radius-full);
  display: flex; align-items: center; gap: 6px; box-shadow: 0 1px 2px rgba(0,0,0,0.02);
}
.remove-tag {
  background: rgba(13, 148, 136, 0.1); border: none; color: var(--c-teal-700);
  border-radius: 50%; width: 20px; height: 20px;
  cursor: pointer; padding: 0; display: flex; align-items: center; justify-content: center;
  transition: all .2s;
}
.remove-tag:hover { background: var(--c-teal-600); color: #fff; }

.select-input {
  border: none; outline: none; background: transparent; font-size: 14px;
  color: var(--c-text); flex: 1 1 10px; min-width: 10px; padding: 6px 0;
}
.select-input::placeholder { color: var(--c-text-mute); }

.chevron-wrapper {
  border-left: 1px solid var(--c-border);
  margin-left: -12px;
  display: flex; align-items: center; justify-content: center; height: 100%;
}
.chevron { color: var(--c-text-mute); transition: transform .2s; flex-shrink: 0; }
.select-trigger.is-open .chevron { transform: rotate(180deg); color: var(--c-teal-600); }

.select-options {
  position: absolute; top: calc(100% + 8px); left: 0; width: 100%; max-height: 280px;
  overflow-y: auto; background: #fff; border: 1px solid var(--c-border);
  border-radius: var(--radius-md); box-shadow: 0 10px 30px rgba(0,0,0,0.1);
  padding: 8px 0; margin: 0; list-style: none; z-index: 100;
}
.select-option {
  display: flex; align-items: center; gap: 12px; padding: 10px 16px;
  font-size: 14px; color: var(--c-text-soft); cursor: pointer; transition: background .2s, color .2s;
}
.select-option:hover { background: var(--c-bg-soft); color: var(--c-text); }
.select-option.is-selected { color: var(--c-teal-700); background: #f3fdfa; font-weight: 600; }
.checkbox {
  width: 18px; height: 18px; border: 1.5px solid var(--c-border-strong); border-radius: 4px;
  display: flex; align-items: center; justify-content: center; color: transparent;
  transition: all .2s;
}
.select-option.is-selected .checkbox { background: var(--c-teal-500); border-color: var(--c-teal-500); color: #fff; }

.select-create { color: var(--c-teal-600); font-weight: 600; justify-content: flex-start; }
.select-create span { opacity: 0.7; font-weight: 400; font-style: italic; margin-left: 4px; font-size: 12.5px; }

.select-options::-webkit-scrollbar { width: 6px; }
.select-options::-webkit-scrollbar-track { background: transparent; }
.select-options::-webkit-scrollbar-thumb { background: var(--c-border-strong); border-radius: 10px; }

.dropdown-enter-active, .dropdown-leave-active { transition: opacity .2s, transform .2s; transform-origin: top; }
.dropdown-enter-from, .dropdown-leave-to { opacity: 0; transform: translateY(-8px) scaleY(0.95); }
</style>
