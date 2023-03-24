<template>
  <nav>
    <h1>{{ title }}</h1>
    <!-- <BasicButton @toggleOverhead="toggleOverhead" text="Lights" color="green" /> -->
    <DarkModeToggle />
  </nav>
</template>

<script>
// import BasicButton from "./BasicButton.vue";
import DarkModeToggle from "./DarkModeToggle.vue";

export default {
  name: "HeaderComp",
  props: {
    title: String,
  },
  components: {
    DarkModeToggle,
  },
  methods: {
    toggleOverhead(data) {
      this.$emit("toggleOverhead", data);
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

h1 {
  font-size: 2em;
  margin: 15px;
}
</style>
