<template>
  <button @click="onClick()" :style="{ background: color }" class="btn">
    {{ text }}
  </button>
</template>

<script>
export default {
  name: "BasicButton",
  props: {
    text: String,
    color: String,
  },
  methods: {
    onClick() {
      this.$emit("toggleOverhead", 0);
      console.log("Clicked Button");
    },
  },
};
</script>
