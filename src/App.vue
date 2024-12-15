<script setup lang="ts">
import { useForm } from 'vee-validate'
import Input from './components/input.vue'
import { toTypedSchema } from '@vee-validate/yup'
import { array, boolean, object, string } from 'yup'
import InputArray from './components/inputArray.vue'

const schema = toTypedSchema(
  object({
    username: string().required(),
    email: string().required().email(),
    password: string().required().min(6),
    student: boolean().required().default(false),
    friends: array(string()),
  }),
)

const { handleSubmit, isSubmitting } = useForm({
  validationSchema: schema,
  initialValues: { friends: ['Hoshang'] },
})

const onSubmit = handleSubmit((values) => {
  return new Promise<void>((resolve) =>
    setTimeout(() => {
      alert(JSON.stringify(values, null, 2))
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
    <h1 class="text-4xl pb-12 text-emerald-500 z-10 font-extrabold tracking-tight">
      Vee Validate <span class="text-xs text-white font-medium">with yup</span>
    </h1>
    <form
      @submit="onSubmit"
      class="flex flex-col gap-6 z-10 w-full max-w-md mx-auto items-center justify-center"
    >
      <Input name="username" label="Username" />
      <Input name="email" label="Email" />
      <Input name="password" label="Password" />
      <InputArray name="friends" label="Friends" />
      <Input name="student" label="Student" type="checkbox" />
      <button
        :disabled="isSubmitting"
        class="w-full bg-emerald-500 rounded-full text-black font-medium disabled:bg-emerald-800 disabled:text-white/50 p-1"
      >
        {{ !isSubmitting ? 'Submit' : 'Submitting...' }}
      </button>
    </form>
    <div class="absolute size-72 blur-3xl z-0 top-44 bg-emerald-900 opacity-20 rounded-full" />
  </div>
</template>
