<template>
  <div id="app" class="small-container">
    <h1>CrossWars</h1>

    <entry-form @add:entry="addEntry" />
    <h3>Your Times</h3>
    <entry-table :entries="entries" />
  </div>
</template>

<script>
import EntryTable from "./components/EntryTable.vue";
import EntryForm from "./components/EntryForm.vue";

export default {
  title: "CrossWars",
  name: "App",
  components: {
    EntryTable,
    EntryForm,
  },
  data() {
    return {
      entries: [],
    };
  },

  mounted() {
    this.getEntries();
  },
  methods: {
    async getEntries() {
      try {
        // using user_id of testdude
        const uri =
          "http://localhost:8080/crossward/api/entries?user_id=123456789";
        this.$http.get(uri).then((result) => {
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

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
</style>
