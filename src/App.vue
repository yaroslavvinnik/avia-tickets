<template>
  <div id="app">
    <div class="container">
      <img class="logo" alt="logo" src="./assets/images/logo.svg">
      <div class="tickets-wrap">
        <FilterTransfer v-on:changeFilter="onFilterChange"/>
        <div class="filters">
          <FilterType v-on:changeSort="onSortChange" />
          <Tickets v-bind:sortedItems="sortedItems"
                   v-bind:sort="sort"/>

          <div class="show-more" v-on:click="() => page = page + 1">Показать еще 5 билетов!</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ky from 'ky';
import dayjs from 'dayjs';
import duration from 'dayjs/plugin/duration';

import FilterTransfer from './components/FilterFransfer.vue';
import FilterType from './components/FilterType.vue';
import Tickets from './components/Tickets.vue';

dayjs.extend(duration);

export default {
  name: 'App',
  components: {
    FilterTransfer,
    FilterType,
    Tickets,
  },
  data() {
    return {
      page: 1,
      filter: [],
      tickets: [],
      sort: 'sortPrice',
    };
  },
  mounted() {
    this.getIdNormalized();
  },
  methods: {
    onSortChange(value) {
      this.sort = value;
    },
    onFilterChange(value) {
      this.filter = value;
    },
    dayjs,
    formatDates(segment) {
      return `${dayjs(segment.date).format('HH:mm')} - ${dayjs(segment.date).add(segment.duration, 'minutes').format('HH:mm')}`;
    },
    formatDuration(time) {
      return dayjs.duration(time, 'minutes').format('HHч mmм');
    },
    async getIdNormalized() {
      const { searchId } = await ky.get('https://front-test.beta.aviasales.ru/search').json();
      this.tickets = await ky.get(`https://front-test.beta.aviasales.ru/tickets?searchId=${searchId}`).json();
      console.log(this.tickets.tickets);
    },
  },
  watch: {
    filter() {
      this.page = 1;
    },
    sort() {
      this.page = 1;
    },
  },
  computed: {
    sortedItems() {
      let { tickets = [] } = this.tickets;

      tickets = tickets.filter((item) => {
        const first = item.segments[0].stops;
        const back = item.segments[1].stops;
        const stops = first.length + back.length;
        return this.filter.length ? this.filter.includes(stops.toString()) : true;
      });

      if (this.sort === 'sortPrice') {
        tickets.sort((a, b) => {
          if (a.price > b.price) {
            return 1;
          }

          if (a.price < b.price) {
            return -1;
          }
          return 0;
        });
      }

      if (this.sort === 'sortTime') {
        tickets.sort((a, b) => {
          const aTime = a.segments[0].duration + a.segments[1].duration;
          const bTime = b.segments[0].duration + b.segments[1].duration;

          console.log(aTime, bTime);

          if (aTime > bTime) {
            return 1;
          }

          if (aTime < bTime) {
            return -1;
          }
          return 0;
        });
      }

      if (this.sort === 'sortOptimal') {
        tickets.sort((a, b) => {
          if ((a.price > b.price) && (a.segments[0].duration + a.segments[1].duration
            < b.segments[0].duration + b.segments[1].duration)) {
            return 1;
          }

          if ((a.price < b.price) && (a.segments[0].duration + a.segments[1].duration
            > b.segments[0].duration + b.segments[1].duration)) {
            return -1;
          }
          return 0;
        });
      }

      tickets = tickets.splice(0, this.page * 5);
      return tickets;
    },
  },
};
</script>

<style lang="scss">
  @import "assets/styles/reset.css";
  @import "assets/styles/palette.css";
  @import "assets/styles/mixin";

  #app {
    font-family: 'Open Sans', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    overflow: hidden;
  }
  .logo {
    display: block;
    width: 60px;
    height: 60px;
    margin: 0 auto;
  }
  .container {
    width: 100%;
    max-width: 786px;
    padding: 50px 16px;
    margin: 0 auto;
  }
  .tickets-wrap {
    display: grid;
    grid-template-columns: 232px 1fr;
    grid-column-gap: 20px;
    margin-top: 50px;

    @include tablet {
      display: block;
    }
  }
  .show-more {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 50px;
    background-color: var(--sky-blue);
    color: var(--main-white);
    font-size: 12px;
    text-transform: uppercase;
    letter-spacing: .5px;
    border-radius: 5px;
  }
</style>
