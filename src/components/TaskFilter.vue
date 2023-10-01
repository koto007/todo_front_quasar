<template>
  <li>
    <q-item-section class="text-center text-xl"
      ><q-icon name="filter_alt"></q-icon
    ></q-item-section>
  </li>

  <li v-for="s in list" :key="s.id" class="each-filter" :class="s.code">
    <button
      class="border border-sky-400 rounded px-2"
      @click="$emit('update:currentFilter', s.code)"
      :class="{
        'bg-sky-400 text-gray-100': currentFilter == s.code,
        'bg-gray-100 text-gray-600': currentFilter != s.code,
      }"
    >
      {{ s.label }}
    </button>
  </li>
</template>

<script>
import { defineComponent } from "vue";
import { STATUS } from "src/constants/constant.js";

export default defineComponent({
  name: "TaskFilter",
  props: {
    currentFilter: String,
    filtersList: Array,
  },
  computed: {
    list() {
      return [
        { id: 0, label: "All", code: STATUS.CODE_ALL },
        ...new Set(this.filtersList),
      ];
    },
  },
});
</script>
