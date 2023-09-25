<template>
  <div class="app py-12" ref="app-container">
    <div class="text-center">
      <TypeWriter />
    </div>
    <div class="filters flex flex-col justify-center items-center">
        <div class="flex flex-col md:flex-row lg:flex-row py-6">
          <span class="ml-2 pr-2 font-medium">Brands:</span>
          <div v-for="brand in listedBrands" :key="brand" class="">
            <input type="checkbox" :id="brand" :value="brand" v-model="selectedBrands" class="mx-2 cursor-pointer" />
            <label :for="brand">{{ brand }}</label>
          </div>
        </div>

        <div class="flex flex-col md:flex-row lg:flex-row pb-6">
          <div class="flex items-center mr-6">
            <span class="mr-2 font-medium">In Stock:</span>
              <div class="relative">
                <input type="checkbox" id="inStockToggle" v-model="inStockOnly" class="cursor-pointer" />
              </div>
          </div>

          <div class="flex items-center">
            <span class="font-medium pr-2">Sort by:</span>
            <select v-model="sortId" class="border rounded p-2">
              <option value="">-</option>
              <option value="1">Relevance</option>
              <option value="2">Price: Low to High</option>
              <option value="3">Price: High to Low</option>
            </select>
          </div>
        </div>
    </div>

    <div class="text-center">
      Number of results: {{filteredAndSortedItems.length}}
    </div>

    <div class="grid sm:grid-cols-2 md:grid-cols-3 lg-grid-cols-4 gap-6 mx-6 py-6">
      <a href="#" class="rounded-md duration-200 hover:scale-105 focus:scale-105 relative overflow-hidden" v-for="(item, index) in filteredAndSortedItems" key="index">
        <ProductGridItem :product="filteredAndSortedItems[index]" />
      </a>
    </div>
  </div>
</template>

<script>
import ProductGridItem from './components/ProductGridItem.vue';
import TypeWriter from './components/TypeWriter.vue'
import products from './data/products.json';

export default {
  name: 'App',
  components: {
    ProductGridItem,
    TypeWriter,
  },
  data() {
    return {
      products,
      selectedBrands: [],
      inStockOnly: false,
      sortId: '',
    };
  },
  computed: {
    filteredAndSortedItems() {
      let result = [...this.products]

      if (this.selectedBrands.length) {
        result = result.filter((item) => this.selectedBrands.includes(item.brand));
      }

      if (this.inStockOnly) {
        result = result.filter((item) => item.isAvailable === true);
      }

      switch (this.sortId) {
        case '1':
          //thanks chatGPT 0.o
          result.sort((a, b) => {
              if (a.isAvailable && !b.isAvailable) {
                  return -1;
              } else if (!a.isAvailable && b.isAvailable) {
                  return 1;
              } else {
                  const aRank = a.rank !== null ? a.rank : Number.MAX_SAFE_INTEGER;
                  const bRank = b.rank !== null ? b.rank : Number.MAX_SAFE_INTEGER;
                  return aRank - bRank;
              }
          });
          break;
        case '2':
          result.sort((a, b) => a.price - b.price);
          break;
        case '3':
          result.sort((a, b) => b.price - a.price);
          break;
      }

      return result;
    },
    listedBrands() {
      const brands = this.products.map(product => product.brand);
    
      return [...new Set(brands)];
    }
  }
};
</script>

<style>
.app {
  max-width: 1024px;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  color: #606569;
}

.link {
  color: #3a7f71;
}

</style>
