<template>
  <div
    class="grid grid-cols-2 sm:grid-cols-2 md:grid-cols-4 lg:grid-cols-4 xl:grid-cols-4 gap-4"
  >
    <nuxt-link :to="product.slug" v-for="(product, i) in products" :key="i">
      <div
        class="h-48 px-2 w-full rounded-lg border-2 border-black mb-1 flex items-center justify-center"
        :class="product.color"
      >
        <img :src="product.image" class="h-32" alt="" />
      </div>
      <p class="font-bold">{{ product.name }}</p>
      <p class="text-sm">{{ product.currency }}{{ product.price }}</p>
    </nuxt-link>
  </div>
</template>

<script>
export default {
  computed: {
    products() {
      return this.$store.state.products;
    }
  }
};
</script>

<style></style>
