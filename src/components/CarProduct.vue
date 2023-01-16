<template>
  <h1>Achat de {{ props.product.type }}</h1>
  <div>
    <div class="grid">
      <button @click="handleClick('add')" :disabled="!props.product.available">
        Ajouter
      </button>
      <button @click="handleClick('remove')" :disabled="count === 0">
        Retirer
      </button>
    </div>
    <p>Vous avez {{ count }} {{ props.product.type }} dans le panier</p>
    <p>Le prix total est de {{ totalPrice }}</p>
  </div>
</template>

<script lang="ts" setup>
import { computed, ref } from "vue";
import type ProductInterface from "@/types/product.interfaces";
const count = ref(0);

const props = defineProps<{
  product: ProductInterface;
}>();

const emit = defineEmits<{
  (e: "getTotal", price: number, action: string): void;
}>();

function handleClick(action: string) {
  if (action === "remove") count.value--;
  else count.value++;
  emit("getTotal", props.product.price, action);
}

const totalPrice = computed(() => {
  return Intl.NumberFormat("fr-FR", {
    style: "currency",
    currency: "EUR",
  }).format(props.product.price * count.value);
});
</script>

<style scoped lang="scss"></style>
