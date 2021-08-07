<template>
  <div id="entry-form">
    <form @submit.prevent="handleSubmit">
      <label>Todays Crossword Time</label>
      <input
        type="text"
        :class="{ 'has-error': submitting && invalidTime }"
        v-model="entry.time"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">
        ❗Please fill out time field
      </p>
      <p v-if="success" class="success-message">✅ Time successfully added</p>
      <button>Add Time</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      entry: {
        time: "",
        user_id: "123456789"
      },
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (!this.validTime) {
        this.error = true;
        return;
      }

      this.$emit("add:entry", this.entry);
      this.entry = {
        time: "",
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    },
  },
  computed: {
    validTime() {
      if (this.entry.time === "") {
        return false;
      }
      // check if time is a valid time: positive integer, no leading zeroes
      var n = Math.floor(Number(this.entry.time));
      return n !== Infinity && String(n) === this.entry.time && n >= 0;
    },
  },
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}
[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>