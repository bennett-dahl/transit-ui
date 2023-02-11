<template>
  <div
    @dblclick="$emit('toggle-reminder', task)"
    :class="[task.reminder ? 'reminder' : '', 'task']"
  >
    <h3>
      {{ task.text }}
      <div @click="$emit('delete-task', task.id)">
        <i class="fas fa-times"></i>
      </div>
    </h3>
    <p>{{ task.day }}</p>
  </div>
</template>

<script>
export default {
  name: "TaskSingle",
  props: {
    task: Object,
  },
};
</script>

<style scope>
.svg-inline--fa {
  color: red;
}
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
