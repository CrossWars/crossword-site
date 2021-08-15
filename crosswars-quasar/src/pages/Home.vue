<template lang="">
  <EntryForm @add:entry="addEntry"/>
  <EntryTable :entries="entries"/>
</template>
<script>
import { Cookies } from "quasar";
import EntryTable from "components/EntryTable.vue";
import EntryForm from "components/EntryForm.vue";

export default {
  name: "Home",
  components: {
    EntryTable,
    EntryForm,
  },
  data() {
    return {
      user_id: "123456789",
      groups: [],
      entries: [],
    };
  },
  mounted() {
    this.getGroups();
    this.getEntriesForGroup();
  },
  methods: {
    async getGroups() {
      try {
        this.$api.get(
          "/groups/users?group_id=-1001392971649"
        )
        .then((result) => {
            this.entries = result.data.reverse();
          });
      }
      catch (error) {
        console.error(error);
      }
    },
    async getEntriesForGroup() {
      try {
        // using user_id of testdude
        this.$api
          .get(
            "/entries/groups?group_id=-1001392971649&from_date=2021-01-01&to_date=2021-01-01"
          )
          .then((result) => {
            this.entries = result.data.reverse();
          });
      } catch (error) {
        console.error(error);
      }
    },
    async addEntry(entry) {
      try {
        // using user_id of testdude
        const uri = "http://localhost:8080/crossward/api/entries";
        this.$http.post(uri, entry).then((result) => {
          this.entries = [result.data, ...this.entries];
        });
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>
<style scoped>
</style>