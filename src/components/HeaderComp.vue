<template>
  <header>
    <h1>{{ title }}</h1>
    <BasicButton text="UI" color="green" />
  </header>
</template>

<script>
import BasicButton from "./BasicButton.vue";

export default {
  name: "HeaderComp",
  props: {
    title: String,
  },
  components: {
    BasicButton,
  },
};
</script>

<style>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
