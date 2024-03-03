<template>
  <section class="calendar">
    <calendar @showDay="showDay"/>
    <day v-if="selectedDay" :day="selectedDay" />
  </section>
</template>

<script>
import calendar from '../cmps/calendar.vue'
import day from '../cmps/day.vue'
export default {
  data() {
    return {
      selectedDay: null,
    };
  },
  computed: {
  },
  created() {
  },
  methods: {
    showDay(day) {
      this.selectedDay = day;
    }
  },
  components: {
    calendar,
    day
  }
};
</script>
