<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      v-show="homdePage"
      @btn-click="$emit('toggle-add-task')"
      :title="showAddTask ? 'Close' : 'Add Task'"
      :color="showAddTask ? 'red' : 'green'"
    />
  </header>
</template>



<script>
import Button from "./Button.vue";

export default {
  name: "Header",
  props: {
    title: {
      type: String,
    },
    showAddTask: Boolean,
  },

  components: {
    Button,
  },
  computed: {
    homdePage() {
      if (this.$route.path === "/") return true;
      return false;
    },
  },
  emits: ["toggle-add-task"],
};
</script>


<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
