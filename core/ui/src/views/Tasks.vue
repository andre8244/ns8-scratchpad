<template>
  <div>
    <h1>Tasks page</h1>
    <InlineNotification
      v-if="error.clusterTasks"
      kind="error"
      title="Cannot retrieve tasks:"
      :sub-title="error.clusterTasks"
      low-contrast
    />
  </div>
</template>

<script>
import TaskService from "@/mixins/task";
import UtilService from "@/mixins/util";
import to from "await-to-js";

export default {
  name: "Tasks",
  mixins: [TaskService, UtilService],
  data() {
    return {
      error: {
        clusterTasks: "",
      },
    };
  },
  mounted() {
    this.retrieveTasks();
  },
  methods: {
    async retrieveTasks() {
      const [taskError, response] = await to(this.getClusterTasks());

      if (taskError) {
        this.error.clusterTasks = this.getErrorMessage(taskError);
        return;
      }

      console.log("tasks", response); ////
    },
  },
};
</script>
