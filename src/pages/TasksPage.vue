<template>
  <q-page>
    <div>
      <div
        class="shadow-md rounded overflow-hidden to-teal-100 p-10 bg-sky-50 mx-5"
      >
        <h2 class="text-center">
          TODO
          <q-item-section style="display: inline-block" class="">
            <q-icon name="check" />
          </q-item-section>
        </h2>
        <task-new @add="add"></task-new>
        <div
          class="flex flex-row justify-start w-full text-center space-y-6 content-center flex-col mb-4"
        >
          <ul class="flex flex-row gap-2">
            <task-filter
              :filters-list="statuses"
              v-model:currentFilter="currentFilter"
            ></task-filter>
          </ul>
        </div>
        <div
          class="flex flex-row justify-start w-full text-center space-y-6 content-center flex-col mb-4"
        >
          <ul class="flex flex-row gap-2">
            <li>
              <q-item-section class="text-center text-2xl"
                ><q-icon name="import_export"></q-icon
              ></q-item-section>
            </li>
            <li>
              <button
                class="border-purple-400 border rounded px-1"
                :class="{
                  'bg-purple-400 text-gray-100': alphabeticalOrderAsc,
                  'bg-gray-100 text-gray-600': !alphabeticalOrderAsc,
                }"
                @click="alphabeticalOrderAsc = true"
              >
                <q-item-section class="text-center text-xl"
                  ><q-icon name="text_rotation_down"></q-icon
                ></q-item-section>
              </button>
            </li>
            <li>
              <button
                class="border-purple-400 border rounded px-1"
                :class="{
                  'bg-purple-400 text-gray-100': !alphabeticalOrderAsc,
                  'bg-gray-100 text-gray-600': alphabeticalOrderAsc,
                }"
                @click="alphabeticalOrderAsc = false"
              >
                <q-item-section class="text-center text-xl"
                  ><q-icon name="text_rotate_up"></q-icon
                ></q-item-section>
              </button>
            </li>
          </ul>
        </div>
        <div
          v-if="filteredTasks"
          class="content-center space-y-6 px-5 p-3 flex items-baseline flex-col text-center border border-rounded-lg border-collapse table-auto w-full text-sms"
        >
          <ul
            class="mb-2 grid align-items-center items-center gap-3"
            style="grid-template-columns: 1fr 5fr 1fr 1fr"
          >
            <one-task
              v-for="task in filteredTasks"
              :key="task.id"
              @toggleStatus="toggleStatus"
              @updateLabel="updateLabel"
              @remove="remove"
              @toggleEditMode="toggleEditMode"
              :task="task"
            ></one-task>
          </ul>
        </div>
      </div>
    </div>
  </q-page>
</template>
<script>
import { defineComponent, ref } from "vue";
import axios from "axios";
import TaskNew from "components/TaskNew.vue";
import OneTask from "components/OneTask.vue";
import TaskFilter from "components/TaskFilter.vue";
import { STATUS } from "src/constants/constant.js";

export default defineComponent({
  name: "TasksPage",
  data() {
    return {
      tasks: [],
      statuses: [],
      editMode: false,
      currentFilter: STATUS.CODE_ALL,
      alphabeticalOrderAsc: true,
    };
  },
  components: { TaskNew, OneTask, TaskFilter },
  computed: {
    filteredTasks() {
      let tasks = this.sortTaskAlphabetically(this.tasks);
      if (this.currentFilter == STATUS.CODE_ALL) return tasks;
      if (this.currentFilter == STATUS.CODE_COMPLETED)
        return tasks.filter((t) => t.completed);
      if (this.currentFilter == STATUS.CODE_IN_PROGRESS)
        return tasks.filter((t) => !t.completed);
      return tasks;
    },
  },
  methods: {
    returnIconNameFromStatus(completed) {
      if (!completed) return "check_box_outline_blank";
      else return "check_box";
    },
    toggleStatus(id, completed) {
      this.updateStatus(id, completed);
    },
    remove(id) {
      axios
        .delete("http://127.0.0.1:8000/api/tasks/" + id)
        .then((response) => {
          this.loadTasks();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    add(newTask) {
      axios
        .post("http://127.0.0.1:8000/api/tasks/", { label: newTask })
        .then(() => {
          this.loadTasks();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    updateLabel(id, value) {
      axios
        .patch("http://127.0.0.1:8000/api/tasks/label/" + id, {
          key: value,
        })
        .then((response) => {
          this.loadTasks();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    updateStatus(id, value) {
      axios
        .patch("http://127.0.0.1:8000/api/tasks/status/" + id, {
          key: value,
        })
        .then((response) => {
          this.loadTasks();
        })
        .catch((error) => {
          console.log(error);
        });
    },

    loadTasks() {
      axios.get("http://127.0.0.1:8000/api/tasks").then(
        (response) => {
          this.tasks = response.data.data;
        },
        (error) => {
          console.log(error);
        }
      );
    },
    toggleEditMode() {
      // this.editMode = !this.editMode;
    },
    loadStatuses() {
      axios.get("http://127.0.0.1:8000/api/statuses").then(
        (response) => {
          this.statuses = [];
          this.statuses = response.data.data;
        },
        (error) => {
          console.log(error);
        }
      );
    },
    sortTaskAlphabetically(list) {
      return list.sort((a, b) => {
        if (this.alphabeticalOrderAsc) {
          if (a.title < b.title) {
            return -1;
          }
          if (a.title > b.title) {
            return 1;
          }
          return 0;
        } else {
          if (a.title > b.title) {
            return -1;
          }
          if (a.title < b.title) {
            return 1;
          }
          return 0;
        }
      });
    },
  },
  mounted() {
    this.loadTasks();
    this.loadStatuses();
  },
});
</script>
