<template>
  <a class="product-grid-item">
    <div v-if="!product.isAvailable" class="absolute top-0 left-0 bg-red-500 text-white text-xs py-1 px-2">Out of Stock</div>
    <img class="product-image" :src="product.image" />
    <p class="product-name font-bold pt-2 pl-2">{{ product.name }}</p>
    <p class="product-name italic pl-2">{{ product.brand }}</p>
    <p class="product-name text-right p-2">£{{ product.price }}</p>
    
  </a>
</template>

<script>
export default {
  name: 'ProductGridItem',
  props: {
    product: { type: Object, required: true, default: () => ({}) },
  },
};
</script>

<style scoped>
.product-grid-item {
  background: #ddebe8;
  max-width: 150px;
  position: relative;
}

.product-image {
  width: 100%;
}
</style>
