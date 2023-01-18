<script setup lang="ts">
import { useField, useForm } from "vee-validate";
import { z } from "zod";
import { toFormValidator } from "@vee-validate/zod";

const schema = z
  .object({
    password: z.string().min(6),
    verifPassword: z.string(),
  })
  .refine((data) => data.password === data.verifPassword, {
    message: "Les mots de passe ne correspondent pas",
    path: ["verifPassword"],
  });

useForm({
  validationSchema: toFormValidator(schema),
});

const { value: passwordValue, errorMessage: errorPassword } = useField("password");
const {
  value: verifPasswordValue,
  errorMessage: errorPasswordVerification,
  handleChange,
} = useField("verifPassword", {}, { validateOnValueUpdate: false });
</script>

<template>
  <form>
    <input type="text" v-model="passwordValue" placeholder="Mot de passe" />
    <p v-if="errorPassword">{{ errorPassword }}</p>
    <input
      type="text"
      v-model="verifPasswordValue"
      placeholder="Verifer le mot de passe"
      @blur="handleChange"
    />
    <p v-if="errorPasswordVerification">{{ errorPasswordVerification }}</p>
  </form>
</template>

<style scoped lang="scss"></style>
