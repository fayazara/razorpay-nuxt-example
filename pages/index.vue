<template>
  <main>
    <hero />
    <div class="container my-4 mx-auto px-4">
      <chips />
      <products />
    </div>
  </main>
</template>

<script>
import hero from "~/components/hero";
import chips from "~/components/chips";
import products from "~/components/products";
export default {
  components: {
    hero,
    chips,
    products
  }
};
</script>
