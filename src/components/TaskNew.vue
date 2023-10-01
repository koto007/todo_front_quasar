<template>
  <form class="shadow-sm bg-white mb-5 p-5 text-center" @submit.prevent="add()">
    <div class="mb-2 flex flex-row items-center justify-center">
      <div>
        <input
          type="text"
          name="newTask"
          class="border border-gray-500 rounded py-1 px-2 mr-1"
          placeholder="Add new task !"
          v-model="newTask"
        />
      </div>
      <div
        class="rounded border border-emerald-400 text-emerald-400 h-full py-1 px-2"
      >
        <button name="add">
          <q-icon name="add" />
        </button>
      </div>
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
