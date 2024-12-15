<!-- eslint-disable vue/multi-word-component-names -->
<script lang="ts" setup>
import { useField, type InputType } from 'vee-validate'
import type { InputTypeHTMLAttribute } from 'vue'

const { label, name, type, placeholder } = defineProps<{
  label?: string
  name: string
  type?: InputTypeHTMLAttribute
  placeholder?: string
}>()

const { value, errorMessage, meta, handleBlur, handleChange } = useField(() => name, undefined, {
  type: type == 'radio' || type == 'checkbox' ? (type as InputType) : 'default',
})
</script>

<template>
  <div
    class="flex w-full"
    :class="type == 'radio' || type == 'checkbox' ? 'flex-row gap-4' : 'flex-col gap-1'"
  >
    <label v-if="label" :for="name" class="font-semibold"
      >{{ label }} <span v-show="meta.required" class="text-red-500 text-xs">*</span></label
    >
    <input
      :type="type"
      :placeholder="placeholder"
      :name
      v-model="value"
      @blur="handleBlur"
      @change="type == 'default' && handleChange"
      class="placeholder-gray-500 accent-emerald-500 text-white/90 border-[1.5px] ring-0 outline-0 focus:brightness-110 py-1 px-2 rounded-lg text-sm"
      :class="{
        'bg-slate-900/40 border-slate-800 focus:border-emerald-500': !meta.touched || meta.valid,
        'bg-red-950/30 border-red-500/70': meta.touched && errorMessage,
      }"
    />
    <p v-if="meta.touched && errorMessage" class="text-sm text-red-500">{{ errorMessage }}</p>
  </div>
</template>
