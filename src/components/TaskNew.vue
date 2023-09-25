<template>
  <form class="shadow-sm bg-white mb-5 p-5" @submit.prevent="add()">
    <div class="text-center">
      <input
        type="text"
        class="shadow appearance-none border border-gray-500 rounded py-1 px-2 mr-1"
        placeholder="Add new task !"
        v-model="newTask"
      />
    </div>
  </form>
</template>

<script>
import { defineComponent } from "vue";
export default defineComponent({
  name: "TaskNew",
  data() {
    return {
      newTask: "",
    };
  },
  methods: {
    add() {
      this.$emit("add", this.newTask);
      this.newTask = "";
    },
  },
});
</script>
