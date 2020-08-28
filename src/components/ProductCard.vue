<template>
  <q-dialog :value="value" @input="$emit('input', $event)">
    <q-card class="my-card">
      <q-img src="https://cdn.quasar.dev/img/chicken-salad.jpg" />
      <q-card-section>
        <div class="row no-wrap items-center">
          <div class="col text-h6 ellipsis">
            <span v-if="!editField.name">{{ stateProduct.name }}</span>
            <q-input v-else type="text" v-model="stateProduct.name" @blur="closeEdit('name')" />
          </div>
          <div class="col-auto text-grey text-caption row no-wrap items-center">
            <q-icon v-if="!editField.name" name="create" size="20px" @click="openEdit('name')" />
            <q-icon v-else name="clear" size="20px" @click="closeEdit('name')" />
          </div>
        </div>
      </q-card-section>
      <q-card-section>
        <div class="row no-wrap items-center">
          <div class="col text-h6 ellipsis">
            <span v-if="!editField.price">{{ stateProduct.price }}р.</span>
            <q-input
              v-else
              type="number"
              v-model.number="stateProduct.price"
              @blur="closeEdit('price')"
            />
          </div>
          <div class="col-auto text-grey text-caption row no-wrap items-center">
            <q-icon v-if="!editField.price" name="create" size="20px" @click="openEdit('price')" />
            <q-icon v-else name="clear" size="20px" @click="closeEdit('price')" />
          </div>
        </div>
      </q-card-section>

      <q-separator />

      <q-card-actions align="right">
        <q-btn v-close-popup flat color="primary" label="Готово" @click="changeProduct" />
        <q-btn v-close-popup flat color="primary" label="Отменить" />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>
<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import { ProductType } from 'src/types';

type EditField = {
  [index: string]: any;
  name: boolean;
  price: boolean;
};
@Component
export default class ProductCard extends Vue {
  @Prop({ required: true })
  public readonly value!: boolean;

  @Prop({ required: true })
  public readonly product!: ProductType;

  editField: EditField = {
    name: false,
    price: false,
  };

  stateProduct = { ...this.product };

  openEdit(field: string): void {
    this.editField[field] = true;
  }

  closeEdit(field: string): void {
    this.editField[field] = false;
  }

  changeProduct() {
    this.$emit('change-product', this.stateProduct);
  }
}
</script>

<style lang="scss">
.my-card {
  width: 500px;
}
</style>
