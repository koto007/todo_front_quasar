<template>
  <li class="flex text-2xl">
    <input
      type="checkbox"
      v-model="currentTask.completed"
      :true-value="1"
      :false-value="0"
      @click="toggleStatus()"
      class="check-box"
    />
  </li>
  <li
    class="text-left text-ml each-task"
    :class="className(currentTask.completed)"
  >
    <input
      type="text"
      :readonly="!editMode"
      v-model="currentTask.title"
      v-on:keyup.enter="updateLabel()"
      class="px-2"
      :class="{
        'bg-sky-50': !editMode,
        'line-through': currentTask.completed,
      }"
      name="inputTitle"
    />
  </li>
  <!-- edit btn -->
  <li v-if="!editMode" class="self-center rounded border border-sky-400 mx-1">
    <button
      class="text-xl edit-title"
      @click="toggleEditMode()"
      name="edit-title"
    >
      <span style="display: inline-block" class="text-sky-400">
        <q-icon
          name="edit"
          data-te-toggle="tooltip"
          data-te-placement="bottom"
          data-te-ripple-init
          data-te-ripple-color="light"
          title="Modifier"
        />
      </span>
    </button>
  </li>
  <!-- update btn-->
  <li
    v-if="editMode"
    class="self-center rounded border border-emerald-400 mx-1"
  >
    <button
      class="text-xl update-title"
      @click="updateLabel()"
      name="update-title"
    >
      <span style="display: inline-block" class="text-emerald-400">
        <q-icon
          name="cached"
          data-te-toggle="tooltip"
          data-te-placement="bottom"
          data-te-ripple-init
          data-te-ripple-color="light"
          title="Update"
        />
      </span>
    </button>
  </li>
  <!-- delete -->
  <li class="self-center rounded border border-pink-400 mx-1">
    <button
      class="text-xl delete"
      name="delete"
      @click="remove(currentTask.id)"
    >
      <span style="display: inline-block" class="text-pink-400">
        <q-icon
          name="delete_outline"
          data-te-toggle="tooltip"
          data-te-placement="bottom"
          data-te-ripple-init
          data-te-ripple-color="light"
          title="Supprimer"
        />
      </span>
    </button>
  </li>
  <toast-message
    v-if="message != ''"
    :msg="message"
    :type="'error'"
  ></toast-message>
</template>

<script>
import { defineComponent } from "vue";
import { STATUS } from "src/constants/constant";
import ToastMessage from "./ToastMessage.vue";

export default defineComponent({
  name: "OneTask",
  components: { ToastMessage },
  emits: ["toggleStatus", "updateLabel", "remove", "toggleEditMode"],
  props: {
    task: Object,
  },
  data() {
    return {
      editMode: false,
      currentTask: Object,
      message: "",
    };
  },
  methods: {
    toggleStatus() {
      // this.currentTask.completed = !this.currentTask.completed;
      if (this.currentTask.completed == 0) this.currentTask.completed = 1;
      else if (this.currentTask.completed == 1) this.currentTask.completed = 0;

      this.$emit(
        "toggleStatus",
        this.currentTask.id,
        this.currentTask.completed
      );
    },
    updateLabel(id, title) {
      if (this.currentTask.title.trim() != "") {
        this.$emit("updateLabel", this.currentTask.id, this.currentTask.title);
        // this.newTask = "";
        this.editMode = false;
      } else {
        this.message = "Task name required";
        setTimeout(() => {
          this.message = "";
        }, 5000);
      }
    },
    remove(id) {
      this.$emit("remove", id);
    },
    returnIconNameFromStatus(completed) {
      if (completed == false) return "check_box_outline_blank";
      else return "check_box";
    },
    toggleEditMode() {
      this.$emit("toggleEditMode");
      this.editMode = !this.editMode;
    },
    className(status) {
      return status
        ? "each-task-" + STATUS.CODE_COMPLETED
        : "each-task-" + STATUS.CODE_IN_PROGRESS;
    },
    test() {
      if (this.currentTask.completed) return true;
      else return false;
    },
  },
  computed: {},
  mounted() {
    this.currentTask = this.task;
  },
});
</script>
