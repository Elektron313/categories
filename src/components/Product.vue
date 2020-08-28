<template>
  <div class="product" @click="openCard">
    <p>{{ value.name }}</p>
    <p>{{ value.price }}р.</p>
  </div>
</template>
<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import { ProductType } from 'src/types';

@Component
export default class Product extends Vue {
  @Prop({ required: true })
  public readonly value!: ProductType;

  openCard(): void {
    this.$emit('open-card', this.value.id);
  }
}
</script>

<style lang="scss">
.product {
  display: flex;
  align-items: center;
  justify-content: center;
  &:hover {
    background-color: grey;
  }
  p {
    margin: 20px;
  }
}
</style>
