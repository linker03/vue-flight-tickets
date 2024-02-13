<script>
import 'bootstrap/dist/css/bootstrap.css';
import axios from 'axios';
import TicketCard from './components/TicketCard.vue';
import Filter from './components/Filter.vue';

export default {
  data() {
    return {
      appliedFilters: {
        priceLow: '',
        priceHigh: '',
        duration: '',
        transfers: '',
      },
      ticketsData: [],
      filteredTicketData: [],
      params: {
        currency: 'rub',
        origin: 'MOW',
        // destination: 'DXB',
        // departure_at: '2024-03',
        // return_at: '2024-04',
        // one_way: 'true',
        // direct: 'false',
        market: 'ru',
        limit: 30,
        page: 1,
        // sorting: 'price',
        // unique: 'false',
        token: import.meta.env.VITE_APP_API_TOKEN,
      },
    };
  },
  methods: {
    getTickets() {
      const searchParams = new URLSearchParams(this.params);
      const url = new URL(
        'http://localhost:5173/aviasales/v3/prices_for_dates'
      );
      url.search = searchParams;
      axios.get(url).then((res) => {
        this.ticketsData = res.data.data;
      });
    },
    getFilteredData() {
      const filteredData = this.ticketsData.filter((ticket) => {
        const priceLow = this.appliedFilters.priceLow
          ? +this.appliedFilters.priceLow < +ticket.price
          : true;
        const priceHigh = this.appliedFilters.priceHigh
          ? +this.appliedFilters.priceHigh > +ticket.price
          : true;
        const duration = this.appliedFilters.duration
          ? +this.appliedFilters.duration > +ticket.duration
          : true;
        const transfers = this.appliedFilters.transfers
          ? +this.appliedFilters.transfers === +ticket.transfers
          : true;
        return priceLow && priceHigh && duration && transfers;
      });

      return filteredData;
    },

    applyFilter(filters) {
      this.appliedFilters = filters;
      this.filteredTicketData = this.getFilteredData();
    },
    resetFilters() {
      this.appliedFilters = {
        priceLow: '',
        priceHight: '',
        duration: '',
        transfers: '',
      };
      this.filteredTicketData = [];
    },
  },
  components: { TicketCard, Filter },
  mounted() {
    this.getTickets();
  },
};
</script>

<template>
  <header></header>

  <main>
    <h1>Билеты</h1>
    <Filter @apply-filters="applyFilter" @reset="resetFilters" />
    <div v-if="!filteredTicketData.length" class="tickets-container">
      <TicketCard
        v-for="ticket in this.ticketsData"
        :ticket="ticket"
        :key="ticket.signature"
      />
    </div>
    <div v-else class="tickets-container">
      <TicketCard
        v-for="ticket in this.filteredTicketData"
        :ticket="ticket"
        :key="ticket.signature"
      />
    </div>
  </main>
</template>

<style scoped>
.tickets-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(325px, 1fr));
  gap: 16px;
}
</style>
