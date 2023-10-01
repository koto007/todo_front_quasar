<template>
  <li class="flex text-2xl">
    <q-icon
      :name="returnIconNameFromStatus(currentTask.completed)"
      type="checkbox"
      v-model="currentTask"
      @click="toggleStatus()"
      data-te-toggle="tooltip"
      data-te-placement="bottom"
      data-te-ripple-init
      data-te-ripple-color="light"
      title="Filter my todos"
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
</template>

<script>
import { defineComponent } from "vue";
import { STATUS } from "src/constants/constant";

export default defineComponent({
  name: "OneTask",
  emits: ["toggleStatus", "updateLabel", "remove", "toggleEditMode"],
  props: {
    task: Object,
  },
  data() {
    return {
      editMode: false,
      currentTask: Object,
    };
  },
  methods: {
    toggleStatus() {
      this.currentTask.completed = !this.currentTask.completed;
      this.$emit(
        "toggleStatus",
        this.currentTask.id,
        this.currentTask.completed
      );
    },
    updateLabel(id, title) {
      this.$emit("updateLabel", this.currentTask.id, this.currentTask.title);
      this.editMode = false;
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
      return status ? STATUS.CODE_COMPLETED : STATUS.CODE_IN_PROGRESS;
    },
  },
  computed: {},
  mounted() {
    this.currentTask = this.task;
  },
});
</script>
