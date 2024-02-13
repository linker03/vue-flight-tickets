<script>
export default {
  data() {
    return {
      filters: { priceLow: '', priceHigh: '', duration: '', transfers: '' },
    };
  },
  methods: {
    resetFilters() {
      this.filters = {
        priceLow: '',
        priceHigh: '',
        duration: '',
        transfers: '',
      };
    },
    resetHandler() {
      this.$emit('reset');
      this.resetFilters();
    },
  },
};
</script>
<template>
  <div class="filter-container">
    <h3>Фильтр</h3>
    <div class="input-group mb-3">
      <span class="input-group-text" id="basic-addon1">Цена от</span>
      <input type="text" class="form-control" v-model="filters.priceLow" />
    </div>
    <div class="input-group mb-3">
      <span class="input-group-text" id="basic-addon1">Цена до</span>
      <input type="text" class="form-control" v-model="filters.priceHigh" />
    </div>
    <div class="input-group mb-3">
      <span class="input-group-text" id="basic-addon1">В пути не более</span>
      <input type="text" class="form-control" v-model="filters.duration" />
    </div>
    <div class="input-group mb-3">
      <span class="input-group-text" id="basic-addon1">Кол-во трансферов</span>
      <input type="text" class="form-control" v-model="filters.transfers" />
    </div>
    <button
      class="btn btn-primary"
      style="margin-right: 8px"
      @click="$emit('apply-filters', filters)"
    >
      Применить
    </button>
    <button class="btn btn-secondary" @click="resetHandler">Сбросить</button>
  </div>
</template>
<style scoped>
.filter-container {
  margin-bottom: 16px;
}
</style>
