<template>
  <div class="q-pa-md">
    <q-table
      title="Leaderboard"
      :rows="entries"
      :columns="columns"
      row-key="name"
    />
  </div>
</template>

<script>
import { defineComponent } from 'vue'
//check out https://codepen.io/smolinari/pen/WNQjVBN?editors=1010
const columns = [
  //{ name: 'pos', align: 'left', label: '#', field: 'pos', sortable: true },
  { name: 'user_name', label: 'Name', field: row => row.user.name, align: "left"},
  { name: 'time', label: 'Time', field: 'time', align: "left", sortable: true},
  //{ name: 'date', label: 'Date', field: 'date' },
  { name: 'groups', label: 'Groups', field: row => row.groups,
   //return list of each group name
   format: (groups, row) => groups.map((group) => group.name).join(', ')
  }
]
export default defineComponent({
  name: "EntryTable",
  props: {
    entries: Array
  },
  data() {
    return {
      columns,
      leaderboard_entries: []
    }
  },
  methods: {
    formatTime(timeInSeconds) {
      const mins = Math.floor(timeInSeconds / 60);
      const secs = timeInSeconds % 60;
      return mins + ":" + (secs < 10 ? "0" : "") + secs;
    },
  },
  computed: {

  }
})
</script>
