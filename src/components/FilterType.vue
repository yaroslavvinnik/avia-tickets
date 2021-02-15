<template>
  <div class="filter-type">
    <div class="filter-type__item"
         v-bind:class="sort === 'sortPrice' ? 'filter-type__item--active': ''"
         v-on:click="onSortChange('sortPrice')">
      <span class="filter-type__label">Самый дешевый</span>
    </div>
    <div class="filter-type__item"
         v-bind:class="sort === 'sortTime' ? 'filter-type__item--active': ''"
         v-on:click="onSortChange('sortTime')">
      <span class="filter-type__label">Самый быстрый</span>
    </div>
    <div class="filter-type__item"
         v-bind:class="sort === 'sortOptimal' ? 'filter-type__item--active': ''"
         v-on:click="onSortChange('sortOptimal')">
      <span class="filter-type__label">Оптимальный</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FilterType',
  data() {
    return {
      sort: 'sortPrice',
    };
  },
  methods: {
    onSortChange(newSort) {
      this.$emit('changeSort', newSort);
      this.sort = newSort;
    },
  },
};
</script>

<style scoped lang="scss">
  @import "../assets/styles/mixin";
  .filter-type {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    border-radius: 5px;
    border: 1px solid var(--border-gray);
    height: 50px;
    background-color: var(--main-white);
    overflow: hidden;
    margin-bottom: 20px;

    @include mobile {
      display: block;
      height: auto;
    }

    &__item {
      display: flex;
      align-items: center;
      justify-content: center;
      background-color: transparent;
      border-right: 1px solid var(--border-gray);
      color: var(--main-black);
      font-size: 12px;
      letter-spacing: .5px;
      text-transform: uppercase;
      transition: background-color .3s ease, color .3s ease;
      cursor: pointer;

      @include mobile {
        padding: 12px 14px;
        border-right: 0;
      }

      &:last-child {
        border-right: 0;
      }

      &--active,
      &:hover {
        background-color: var(--sky-blue);
        color: var(--main-white);
      }
    }
  }
</style>
