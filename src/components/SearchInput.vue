<template>
  <div class="search">
    <q-input rounded outlined :value="value" @input="change">
      <template v-slot:append>
        <q-avatar>
          <q-icon name="search" size="large" />
        </q-avatar>
      </template>
    </q-input>
  </div>
</template>
<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';

@Component
export default class SearchInput extends Vue {
  @Prop({ required: true })
  public readonly value!: string;

  change(text: string) {
    this.$emit('input', text);
  }
}
</script>
<style>
.search {
  width: 700px;
}
</style>
