<template>
  <div class="container">
    <CarProduct @get-total="setTotal" :product="productBerline" />
    <hr />
    <CarProduct @get-total="setTotal" :product="productSUV" />
    <hr />
    <p>Vous avez {{ totalCount }} véhicules dans le panier</p>
    <p>Le prix total est de {{ formattedPrice }}</p>
  </div>
</template>

<script setup lang="ts">
import { computed, reactive, ref } from "vue";
import type ProductInterface from "@/types/product.interfaces";
import CarProduct from "@/components/CarProduct/CarProduct.vue";

const productBerline = reactive<ProductInterface>({
  available: true,
  type: "Berline",
  price: 10000,
});
const productSUV = reactive<ProductInterface>({
  available: true,
  type: "SUV",
  price: 2000,
});

const totalCount = ref(0);
const totalPrice = ref(0);

const formattedPrice = computed(() => {
  return Intl.NumberFormat("fr-FR", {
    style: "currency",
    currency: "EUR",
  }).format(totalPrice.value);
});

function setTotal(price: number, action: string) {
  if (action === "remove") {
    totalCount.value--;
    totalPrice.value -= price;
  } else {
    totalCount.value++;
    totalPrice.value += price;
  }
}
</script>

<style scoped lang="scss"></style>
