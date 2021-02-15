<template>
  <div>
    <div class="filters-mob-btn"
         v-on:click="onShowFilters">
      Выберите количество пересадок
    </div>
    <div class="overlay" v-bind:class="showFilters ? 'show' : ''"
         v-on:click="onShowFilters"></div>
    <div class="filters" v-bind:class="showFilters ? 'show' : ''">
      <div class="filters__title">Количество пересадок</div>
      <div class="filters__list">
        <div class="filters__list-item check-box">
          <input class="check-box__input" type="checkbox" id="check" name="check"
                 :checked="all"
                 v-on:change="onFilterChange('all')"/>
          <label class="check-box__label" for="check">
          <span class="check-box__marker">
            <img class="check-box__icon" src="../assets/images/check.svg" alt="check-icon">
          </span>
            Все
          </label>
        </div>

        <div class="filters__list-item check-box">
          <input class="check-box__input" type="checkbox" id="check1" name="check"
                 v-model="filter"
                 value="0"
                 v-on:change="onFilterChange('0')"/>
          <label class="check-box__label" for="check1">
          <span class="check-box__marker">
            <img class="check-box__icon" src="../assets/images/check.svg" alt="check-icon">
          </span>
            Без пересадок
          </label>
        </div>

        <div class="filters__list-item check-box">
          <input class="check-box__input" type="checkbox" id="check2" name="check"
                 v-model="filter"
                 value="1"
                 v-on:change="onFilterChange('1')"/>
          <label class="check-box__label" for="check2">
          <span class="check-box__marker">
            <img class="check-box__icon" src="../assets/images/check.svg" alt="check-icon">
          </span>
            1 пересадка
          </label>
        </div>

        <div class="filters__list-item check-box">
          <input class="check-box__input" type="checkbox" id="check3" name="check"
                 v-model="filter"
                 value="2"
                 v-on:change="onFilterChange('2')"/>
          <label class="check-box__label" for="check3">
          <span class="check-box__marker">
            <img class="check-box__icon" src="../assets/images/check.svg" alt="check-icon">
          </span>
            2 пересадки
          </label>
        </div>

        <div class="filters__list-item check-box">
          <input class="check-box__input" type="checkbox" id="check4" name="check"
                 v-model="filter"
                 value="3"
                 v-on:change="onFilterChange('3')"/>
          <label class="check-box__label" for="check4">
          <span class="check-box__marker">
            <img class="check-box__icon" src="../assets/images/check.svg" alt="check-icon">
          </span>
            3 пересадки
          </label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FilterTransfer',
  data() {
    return {
      all: true,
      filter: [],
      showFilters: false,
    };
  },
  methods: {
    onShowFilters() {
      this.showFilters = !this.showFilters;
    },
    onFilterChange(newFilter) {
      if (newFilter === 'all') {
        if (!this.filter.length) {
          this.filter = ['0', '1', '2', '3'];
          this.all = false;
        } else {
          this.filter = [];
          this.all = true;
        }
      } else {
        this.all = false;
      }
      this.$emit('changeFilter', this.filter);
    },
  },
};
</script>

<style scoped lang="scss">
  @import "../assets/styles/mixin";
  .filters-mob-btn {
    display: none;
    justify-content: center;
    align-items: center;
    height: 50px;
    background-color: var(--sky-blue);
    color: var(--main-white);
    font-size: 12px;
    text-transform: uppercase;
    letter-spacing: .5px;
    border-radius: 5px;
    margin-bottom: 16px;

    @include tablet {
      display: flex;
    }
  }

  .overlay {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, .4);
    z-index: 2;

    &:before {
      display: block;
      content: "\2715";
      position: absolute;
      right: 16px;
      top: 24px;
      width: 24px;
      font-size: 26px;
      color: var(--main-white);
    }
    @include tablet {
      &.show {
        display: block;
      }
    }
  }

  .filters {
    padding: 20px 0;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    background: var(--main-white);
    align-self: flex-start;

    @include tablet {
      position: fixed;
      left: 0;
      top: 160px;
      transform: translateX(-100%);
      transition: transform .3s ease;
      z-index: 3;

      &.show {
        transform: translateX(0);
      }
    }

    &__title {
      font-size: 12px;
      letter-spacing: .5px;
      padding: 0 20px;
      margin-bottom: 10px;
      text-transform: uppercase;
    }

    &__list-item {
      background-color: transparent;
      transition: background-color .3s ease;
    }

    &__list-item:hover {
      background-color: var(--light-blue);
    }
  }

  .check-box {
    &__label {
      display: flex;
      align-items: center;
      padding: 10px 20px;
      cursor: pointer;
      font-size: 13px;
      font-weight: 400;
    }

    &__marker {
      display: block;
      border: 1px solid var(--dark-gray);
      height: 20px;
      width: 20px;
      border-radius: 2px;
      margin-right: 10px;
      position: relative;
    }

    &__icon {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      opacity: 0;
      transition: opacity .3s ease;
    }

    &__input {
      display: none;

      &:checked + .check-box__label {
        .check-box__marker {
          border-color: var(--sky-blue);
        }

        .check-box__icon {
          opacity: 1;
        }
      }
    }
  }
</style>
