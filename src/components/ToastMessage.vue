<template>
  <Transition>
    <div
      class="toast p-3 fixed"
      :class="{
        'bg-pink-400 text-gray-200': type == 'error',
        'bg-blue-400 text-gray-200': type == 'success',
      }"
    >
      {{ msg }}
    </div>
  </Transition>
</template>

<script>
import { defineComponent } from "vue";
export default defineComponent({
  name: "ToastMessage",
  props: {
    msg: String,
    type: String,
  },
});
</script>

<style>
.toast {
  right: 0;
  top: 5rem;
  transform: translateX(-50%);
  transition: opacity 3s ease;
}
</style>
