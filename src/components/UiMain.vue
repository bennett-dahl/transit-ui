<template>
  <div>
    <!-- <div :key="task.id" v-for="task in tasks">
        <TaskSingle
          @toggle-reminder="toggleReminder"
          @delete-task="deleteTask"
          :task="task"
        />
      </div> -->
  </div>
</template>

<script>
//import TaskSingle from "./TaskSingle.vue";
export default {
  //components: { TaskSingle },
  name: "UiMain",
  props: {
    //   tasks: Array,
  },
  methods: {
    //   deleteTask(id) {
    //     this.$emit("delete-task", id);
    //   },
    //   toggleReminder(id) {
    //     console.log("tasksall toggle reminder " + JSON.stringify(id));
    //     this.$emit("toggle-reminder", id);
    //   },
  },
  // emits: ["delete-task", "toggle-reminder"],
};
</script>
