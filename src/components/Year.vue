<template>
<div>
  <h2>{{ year }}</h2>
  <div class="year-day-grid">
    <!-- day of week labels -->
    <div v-for="l in 7" class="day label">{{ dayOfWeekName(l).slice(0, 2).toUpperCase() }}</div>
    <!-- offset days -->
    <div v-for="o in dayOfWeekOffset" class="day offset"></div>
    <!-- days in current year with month initials -->
    <template v-for="m in 12">
      <div
        v-for="(d,i) in daysInMonth(m)"
        class="day"
        :class="{
          start: i == 0,
          success: statusData[getDate(year, m, d)] == 1,
          fail: statusData[getDate(year, m, d)] == -1
        }"
      >
        <span v-if="i == 0">{{ monthInitial(m) }}</span>
      </div>
    </template>
  </div>
</div>
</template>

<script>
export default {
  props: {
    year: Number,
    statusData: Object,
  },
  methods: {
    // build date format yyyy-mm-dd
    getDate: function(year, month, day) {
      return year + '-' + ('0' + month).slice(-2) + '-' + ('0' + day).slice(-2)
    },
    // compute the number of days of the given month
    daysInMonth: function(month) {
      return new Date(this.year, month, 0).getDate();
    },
    // return the month initial
    monthInitial: function(monthIndex) {
      return ['J','F','M','A','M','J','J','A','S','O','N','D'][monthIndex-1];
    },
    // return the day of week name
    dayOfWeekName: function(dayIndex) {
      return ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'][dayIndex-1];
    },
  },
  computed: {
    // compute the offset of weekdays before actual days
    dayOfWeekOffset: function() {
      return new Date(this.year, 0, 1).getDay()
    },
  }
}
</script>

<style>
.year-day-grid {
  display: flex;
  flex-flow: column wrap;
  align-content: center;
  height: calc((10px + 14px) * 7);
}
.year-day-grid .day {
  width: 18px;
  height: 10px;
  line-height: 10px;
  font-size: .7em;
  font-weight: bold;
  padding: 5px 0;
  margin: 2px;
  background: var(--c-background-element);
  position: relative;
  border-radius: 2px;
  transition: all 0.2s;
}
.year-day-grid .day.label{
  margin-right: 10px;
  background: none;
}
.year-day-grid .day.offset {
  background: none;
}
.year-day-grid .day.success {
  color: white;
  background-image: linear-gradient(to bottom right, var(--c-accent) 0, var(--c-accent-variant) 100%);
  background-color: var(--c-accent);
  box-shadow: 0 8px 20px -8px var(--c-shadow);
}
.year-day-grid .day.fail {
  color: var(--c-shadow);
  background: transparent;
}
</style>