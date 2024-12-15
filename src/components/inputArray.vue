<script lang="ts" setup>
import { useFieldArray } from 'vee-validate'
import { computed } from 'vue'

const { label, name } = defineProps<{
  label?: string
  name: string
}>()

const { fields, push, remove } = useFieldArray<string>(name)

const isLastFieldEmpty = computed(() => {
  if (fields.value.length === 0) return false
  const lastField = fields.value[fields.value.length - 1]
  return lastField.value === ''
})
</script>

<template>
  <div class="flex flex-col w-full gap-2">
    <label v-if="label" for="email" class="font-semibold">{{ label }}</label>
    <div v-for="(field, index) in fields" :key="field.key" class="flex w-full gap-1">
      <input
        class="w-full flex-1 bg-slate-900/40 placeholder-gray-500 accent-emerald-500 text-white/90 border-[1.5px] ring-0 outline-0 border-slate-800 focus:brightness-110 focus:border-emerald-500 py-1 px-2 rounded-lg text-sm"
        v-model="field.value"
      /><button
        @click="remove(index)"
        class="border border-slate-800 hover:bg-slate-900/40 text-sm px-2 rounded-lg"
      >
        delete
      </button>
    </div>
    <button
      @click="push('')"
      type="button"
      :disabled="isLastFieldEmpty"
      class="border border-slate-800 disabled:text-white/30 disabled:hover:bg-transparent hover:bg-slate-900/40 text-sm px-2 py-1 rounded-lg"
    >
      add
    </button>
  </div>
</template>
