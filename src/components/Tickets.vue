<template>
  <div>
  <div class="ticket" v-for="(ticket, i) in sortedItems" :key="i">
    <div class="ticket__top">
      <div class="ticket__price">{{ticket.price}} Р</div>
      <div class="ticket__logo">
        <img :src="`//pics.avs.io/99/36/${ticket.carrier}.png`">
      </div>
    </div>
    <div class="ticket__details" v-for="(segment, i) in ticket.segments" :key="i">
      <div class="ticket__departure">
        <div class="ticket__label">{{segment.origin}} – {{segment.destination}}</div>
        <div class="ticket__value">{{formatDates(segment)}}</div>
      </div>
      <div class="ticket__time">
        <div class="ticket__label">В пути</div>
        <div class="ticket__value">
          {{formatDuration(segment.duration)}}</div>
      </div>
      <div class="ticket__transfers">
        <div class="ticket__label" v-if="segment.stops.length === 0">без пересадок</div>
        <div class="ticket__label"
             v-if="segment.stops.length !== 0">
          {{segment.stops.length}} пересадки
        </div>
        <div class="ticket__value">
          <span v-for="(stopItem, i) in segment.stops" :key="i">{{stopItem}} </span>
        </div>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import dayjs from 'dayjs';
import duration from 'dayjs/plugin/duration';

dayjs.extend(duration);
export default {
  name: 'Tickets',
  props: {
    sortedItems: Array,
    sort: String,
  },
  data() {
    return {
      tickets: [],
    };
  },
  methods: {
    dayjs,
    formatDates(segment) {
      return `${dayjs(segment.date).format('HH:mm')} - ${dayjs(segment.date).add(segment.duration, 'minutes').format('HH:mm')}`;
    },
    formatDuration(time) {
      return dayjs.duration(time, 'minutes').format('HHч mmм');
    },
  },
};
</script>

<style scoped lang="scss">
  @import "../assets/styles/mixin";

  .ticket {
    background-color: var(--main-white);
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    padding: 20px 20px 10px;
    margin-bottom: 20px;

    &__top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
    }

    &__price {
      font-size: 24px;
      color: var(--sky-blue);
    }

    &__logo {
      img {
        width: 110px;
        height: auto;
      }
    }

    &__details {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      margin-bottom: 10px;

      @include mobile {
        display: block;
        margin-bottom: 16px;
      }
    }

    &__departure,
    &__time,
    &__transfers {
      @include mobile {
        display: flex;
        align-items: center;
        margin-bottom: 6px;
      }
    }

    &__label {
      font-size: 12px;
      letter-spacing: .5px;
      text-transform: uppercase;
      color: var(--light-gray);
      line-height: 18px;

      @include mobile {
        min-width: 110px;
      }
    }

    &__value {
      font-size: 14px;
      line-height: 21px;
      text-transform: uppercase;

      @include mobile {
        flex-grow: 1;
      }
    }
  }
</style>
