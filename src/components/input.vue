<!-- eslint-disable vue/multi-word-component-names -->
<script lang="ts" setup>
import { useField, type InputType } from 'vee-validate'

const { label, name, type, placeholder } = defineProps<{
  label?: string
  name: string
  type?: InputType
  placeholder?: string
}>()

const { value, errorMessage, meta } = useField(() => name, undefined, {
  type: type ?? 'default',
})
</script>

<template>
  <div class="flex w-full" :class="type ? 'flex-row gap-4' : 'flex-col gap-1'">
    <label v-show="label" for="email" class="font-semibold"
      >{{ label }} <span v-show="meta.required" class="text-red-500 text-xs">*</span></label
    >
    <input
      :type="type ?? 'text'"
      :placeholder="placeholder"
      v-model="value"
      class="bg-slate-900/40 placeholder-gray-500 accent-emerald-500 text-white/90 border-[1.5px] ring-0 outline-0 border-slate-800 focus:brightness-110 focus:border-emerald-500 py-1 px-2 rounded-lg text-sm"
      :class="{
        'bg-red-950/30 border-red-500/70 focus:border-red-500/70': !meta.valid && meta.touched,
      }"
    />
    <p :v-if="errorMessage && meta.touched" class="text-sm text-red-500">{{ errorMessage }}</p>
  </div>
</template>
