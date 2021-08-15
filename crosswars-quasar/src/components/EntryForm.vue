<template lang="">
<div id="entry-form">
    <form @submit.prevent="handleSubmit">
       <q-input 
       outlined 
       v-model="entry.time" 
       label="Enter Your Time" 
       class="inputField"
       mask="#:##"
       fill-mask="0"
       reverse-fill-mask
       @focus="onFocus" />

      <p v-if="error" class="error-message">
        ❗Please fill out time field
      </p>
      <p v-if="success" class="success-message">✅ Time successfully added</p>
      <q-btn 
      color="primary" 
      type="submit"
      label="Submit" 
      class="submitButton"
      
      :loading="submitting">
        <template v-slot:loading>
          <q-spinner-dots />
        </template>
    </q-btn>
    </form>
  </div>
</template>
<script>
import { ref } from "vue";

export default {
  name: "EntryForm",
  setup() {
    const submitting = ref(false);
  },
  methods: {
    enterTime() {
      console.log(this.entry.time);
    },
    validTime() {
      if (this.entry.time === "") {
        return false;
      }
      // check if time is a valid time: positive integer, no leading zeroes
      var n = Math.floor(Number(this.entry.time));
      return n !== Infinity && String(n) === this.entry.time && n >= 0;
    },
    handleSubmit() {
      this.submitting = true;
      this.success = false;

      if (!this.validTime()) {
        this.error = true;
        this.submitting = false;
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
    onFocus() {
        const val = this.entry.time;
        this.entry.time = null;
        this.entry.time = val;
    }
  },
  data() {
    return {
      error: false,
      success: false,
      submitting: false,
      entry: {
        time: "",
        user_id: "123456789",
      },
    };
  },
  computed: {
    
  },
};
</script>
<style scoped>
.inputField {
  margin: 2rem;
}
.submitButton {
  margin-left: 2rem;
}
form {
  margin-bottom: 2rem;
  max-width: 300px;
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