<script>
export default {
  props: {
    ticket: Object,
  },
  data() {
    return {
      departureAt: new Date(this.ticket.departure_at),
    };
  },
  methods: {
    getReadableDate(dateIso) {
      const date = new Date(dateIso);
      const day = date.getDate() < 10 ? `0${date.getDate()}` : date.getDate();
      const month =
        date.getMonth() + 1 < 10
          ? `0${date.getMonth() + 1}`
          : date.getMonth() + 1;
      const hours =
        date.getHours() < 10 ? `0${date.getHours()}` : date.getHours();
      return `${day}-${month}-${date.getFullYear()} ${hours}:${date.getMinutes()}`;
    },
  },
};
</script>
<template>
  <div class="card">
    <div class="card-body">
      <h5 class="card-title">{{ ticket.price.toLocaleString() + ' Р' }}</h5>
      <p class="card-text">Время в пути {{ ticket.duration }} минут</p>
      <p class="card-text">Авиаперевозчик: {{ ticket.airline }}</p>

      <p class="card-text">
        Вылет из: {{ ticket.origin }}/{{ ticket.origin_airport }}
      </p>
      <p class="card-text">
        Время вылета: {{ getReadableDate(ticket.departure_at) }}
      </p>
      <p class="card-text">
        Город и аэропорт назначеничения: {{ ticket.destination }}/{{
          ticket.destination_airport
        }}
      </p>

      <p class="card-text">
        Время полета до места: {{ ticket.duration_to }} минут
      </p>
      <p class="card-text">
        Время полета обратно: {{ ticket.duration_back }} минут
      </p>
      <p class="card-text">
        Пересадок по пути следования: {{ ticket.transfers }}
      </p>
      <p class="card-text">Пересадок обратно: {{ ticket.return_transfers }}</p>
      <a
        :href="'https://www.aviasales.ru' + ticket.link"
        class="btn btn-primary"
        >Перейти к билету</a
      >
    </div>
  </div>
</template>
<style scoped></style>
