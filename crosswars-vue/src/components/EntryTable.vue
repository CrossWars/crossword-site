<template>
  <div id="entry-table">
    <table>
      <thead>
        <tr>
          <th>Date</th>
          <th>Time</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="entry in entries" :key="entry.date">
          <td>{{ entry.date }}</td>
          <td>{{ formatTime(entry.time) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "employee-table",
  props: {
    entries: Array,
  },
  methods: {
    formatTime(timeInSeconds) {
      const mins = Math.floor(timeInSeconds / 60);
      const secs = timeInSeconds % 60;
      return mins + ":" + (secs < 10 ? "0" : "") + secs;
    },
  },
};
</script>
