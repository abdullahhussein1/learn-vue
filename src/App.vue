<script setup lang="ts">
import { useForm } from 'vee-validate'
import { ref } from 'vue'
import * as yup from 'yup'

const limit = ref(5)

const schema = yup.lazy(() =>
  yup.object({
    email: yup.string().required().email(),
    password: yup.string().required('Password is Required').min(limit.value),
  }),
)

const { errors, defineField, meta, handleSubmit, isSubmitting } = useForm({
  validationSchema: schema,
})

const [email, emailAttrs] = defineField('email')
const [password, passwordAttrs] = defineField('password')

const onSubmit = handleSubmit((values, { resetForm }) => {
  return new Promise<void>((resolve) =>
    setTimeout(() => {
      alert(JSON.stringify(values, null, 2))
      resetForm()
      resolve()
    }, 2000),
  )
})
</script>

<template>
  <div
    id="app"
    class="bg-slate-950 min-h-screen flex flex-col text-white items-center justify-center"
  >
    <h1 class="text-4xl pb-12 text-emerald-500 font-extrabold tracking-tight">
      Vee Validate <span class="text-xs text-white font-medium">with yup</span>
    </h1>

    <form
      @submit="onSubmit"
      class="flex flex-col gap-6 w-full max-w-md mx-auto items-center justify-center"
    >
      <div class="flex flex-col gap-1 w-full">
        <label for="email" class="font-semibold"
          >Email <span class="text-red-500 text-xs">*</span></label
        >
        <input
          v-model="email"
          v-bind="emailAttrs"
          :disabled="meta.pending"
          class="bg-slate-900/40 text-white/90 border-[1.5px] ring-0 outline-0 focus:border-emerald-500 hover:bg-slate-900/60 border-slate-800 py-1 px-2 rounded-lg text-sm"
        />
        <p class="text-sm text-red-500">{{ errors.email }}</p>
      </div>
      <div class="flex flex-col gap-1 w-full">
        <label for="password" class="font-semibold"
          >Password <span class="text-red-500 text-xs">*</span></label
        >
        <div class="flex gap-2">
          <input
            v-model="password"
            type="password"
            v-bind="passwordAttrs"
            :disabled="meta.pending"
            class="bg-slate-900/40 text-white/90 flex-1 outline-0 focus:border-emerald-500 border-[1.5px] ring-0 outline-emerald-500 hover:bg-slate-900/60 border-slate-800 py-1 px-2 rounded-lg text-sm"
          />
          <input
            type="number"
            v-model.number="limit"
            :disabled="meta.pending"
            class="bg-slate-900/40 text-white/90 ring-0 outline-0 focus:border-emerald-500 border-[1.5px] outline-emerald-500 hover:bg-slate-900/60 border-slate-800 py-1 px-2 rounded-lg text-sm"
          />
        </div>
        <p class="text-sm text-red-500">{{ errors.password }}</p>
      </div>
      <button
        :disabled="!meta.dirty || !meta.valid || isSubmitting"
        class="w-full bg-emerald-500 rounded-full disabled:bg-emerald-800 disabled:text-white/50 p-1"
      >
        {{ !isSubmitting ? 'Submit' : 'Submitting...' }}
      </button>
    </form>
    <div class="absolute size-72 blur-3xl top-44 bg-emerald-900 opacity-20 rounded-full" />
  </div>
</template>
