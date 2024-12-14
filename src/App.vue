<script setup lang="ts">
import { Form, Field } from 'vee-validate'

const onSubmit = (values) => {
  console.log(values)
}
</script>

<template>
  <div
    id="app"
    class="bg-slate-950 min-h-screen flex flex-col text-emerald-500 items-center justify-center"
  >
    <Form @submit="onSubmit">
      <Field type="email" name="email" />
      <button>Sign up for newsletter</button>
    </Form>
  </div>
</template>
