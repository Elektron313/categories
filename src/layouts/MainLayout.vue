<template>
  <div class="app">
    <div class="header">
      <search-input v-model="search" />
    </div>
    <div class="catalogs">
      <q-list>
        <q-expansion-item
          v-for="category in filterCategories"
          :key="`category_${category.id}`"
          expand-separator
          :label="category.title"
          default-opened
        >
          <product
            v-for="productId in category.products"
            :key="`product_${productId}`"
            :value="getProductById(productId)"
            @open-card="createCard"
          />
        </q-expansion-item>
      </q-list>
      <product-card
        v-if="card"
        :product="getProductById(currentCard)"
        v-model="card"
        @change-product="changeProduct"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';
import { Category, ProductType } from 'src/types';
import SearchInput from 'components/SearchInput.vue';
import ProductCard from 'components/ProductCard.vue';
import allProducts from '../products';
import categories from '../categories';
import Product from '../components/Product.vue';

@Component({
  components: {
    ProductCard,
    Product,
    SearchInput,
  },
})
export default class MainLayout extends Vue {
  search = '';

  products: ProductType[] = allProducts;

  categories: Category[] = categories;

  card = false;

  currentCard = '';

  createCard(productId: string): void {
    this.card = true;
    this.currentCard = productId;
  }

  changeProduct(currentProduct: ProductType): void {
    const index = this.products.findIndex((product) => product.id === currentProduct.id);
    this.products[index] = currentProduct;
  }

  get filterCategories() {
    return this.categories.reduce((acc: Category[], category) => {
      const products: string[] = [];
      category.products.forEach((productId) => {
        const product = this.getProductById(productId);
        if (product) {
          if (product.name.toLowerCase().includes(this.search.toLowerCase())) {
            products.push(productId);
          }
        }
      });
      if (products.length > 0) {
        acc.push({ ...category, products });
      }
      return acc;
    }, []);
  }

  getProductById(id: string): ProductType | undefined {
    return this.products.find((product) => product.id === id);
  }
}
</script>
<style lang="scss">
.app {
  height: 100%;
  .header {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 15px;
  }
  .catalogs {
    height: calc(100% - 86px);
  }
}
</style>
