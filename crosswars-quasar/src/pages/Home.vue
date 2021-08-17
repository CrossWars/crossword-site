<template lang="">
  <EntryForm @add:entry="addEntry"/>
  <EntryTable :entries="leaderboardEntries"/>

</template>
<script>
import EntryTable from "components/EntryTable.vue";
import EntryForm from "components/EntryForm.vue";

export default {
  name: "Home2",
  components: {
    EntryTable,
    EntryForm,
  },
  data() {
    return {
      user_id: "123456789",
      groupIdToGroup: new Map,
      userIdToUser: new Map,
      groupToEntries: new Map(),
      groupToUsers: new Map(),
      leaderboardEntries: [],
    };
  },
  mounted() {
    this.getDataForLeaderboardEntries();
  },
  methods: {
    async getDataForLeaderboardEntries() {
      this.$api.get(`/groups?user_id=${this.user_id}`).then((result) => {
        this.groupIdToGroup = this.arrayToMap(result.data, 'id');
        //[array of user requests for each group]
        let promises = [];
        //let groupToUsers = new Map();
        //let groupToEntries = new Map();
        //See https://stackoverflow.com/questions/56532652/axios-get-then-in-a-for-loop
        for (let group of result.data) {
          //get uses of group, put in groupToUsers map
          promises.push(
            this.$api
              .get(`/groups/users?group_id=${group.id}`)
              .then((usersResponse) => {
                for (let user of usersResponse.data){
                  this.userIdToUser.set(user.user_id, user)
                }
              })
          );
          //get entries of group, put in groupToEntries map
          promises.push(
            //fromDate and toDate are for testing, remove before PR!
            this.$api
              .get(
                `/entries/groups?group_id=${group.id}&from_date=2020-01-01&to_date=2020-01-01`
              )
              .then((entryResponse) => {
                this.groupToEntries.set(group.id, entryResponse.data);
              })
          );
        }
        Promise.all(promises).then(() => {
          // console.log("Groups: ")
          // console.log(this.groupIdToGroup);
          // console.log("Users by Group: ")
          // console.log(this.groupToUsers);
          // console.log("Entries by Group: ")
          // console.log(this.groupToEntries);
          this.createLeaderboardEntries();
        });
      });
    },
    createLeaderboardEntries() {
      //add User names and groups to each entry
      //console.log(this.groupToEntries.get("123456789"));
      let userToLeaderboardEntry = new Map()
      for (let [group_id, group] of this.groupIdToGroup) {
        for (let entry of this.groupToEntries.get(group_id)) {
          //userID not in leaderboardEntry dict, so add it
          if (!userToLeaderboardEntry.has(entry.user_id)) {
            var leaderboardEntry = {
              user: this.userIdToUser.get(entry.user_id),
              groups: [group],
              date: entry.date,
              time: entry.time
            }
            userToLeaderboardEntry.set(entry.user_id, leaderboardEntry);
          } else {
            //entry already exists for this user, just add group to entry
            userToLeaderboardEntry.get(entry.user_id).groups.push(group);
          }
        }
      }
      this.leaderboardEntries = Array.from(userToLeaderboardEntry.values());
      console.log("Leaderboard Entries: ")
      console.log(this.leaderboardEntries);
    },
    arrayToMap(arr, keyField) {
      /* takes an array of objects and makes it into a map based on a key
      [{id: 1, name: Rob}] -> {1: {id: 1, name: rob}}
      */
      var map = new Map();
      for (let obj of arr) {
        map.set(obj[keyField], obj);
      }
      return map;
    },
  },
};
</script>
<style scoped>
</style>