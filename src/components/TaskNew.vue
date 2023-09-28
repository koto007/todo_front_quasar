<template>
  <form class="shadow-sm bg-white mb-5 p-5" @submit.prevent="add()">
    <div class="text-center">
      <input
        type="text"
        class="shadow appearance-none border border-gray-500 rounded py-1 px-2 mr-1"
        placeholder="Add new task !"
        v-model="newTask"
      />
      <toast-message
        v-if="message != ''"
        :msg="message"
        :type="'error'"
      ></toast-message>
    </div>
  </form>
</template>

<script>
import { defineComponent } from "vue";
import ToastMessage from "./ToastMessage.vue";

export default defineComponent({
  name: "TaskNew",
  components: { ToastMessage },
  data() {
    return {
      newTask: "",
      message: "",
      test: String,
    };
  },
  methods: {
    add() {
      if (this.newTask) {
        this.$emit("add", this.newTask);
        this.newTask = "";
      } else {
        this.message = "Task name required";
        setTimeout(() => {
          this.message = "";
        }, 5000);
      }
    },
  },
});
</script>
