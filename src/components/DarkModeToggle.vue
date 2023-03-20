<template>
  <div class="theme-toggle">
    <input type="checkbox" id="toggle_checkbox" @change="toggleTheme" />
    <label for="toggle_checkbox">
      <div id="star">
        <div class="star" id="star-1">★</div>
        <div class="star" id="star-2">★</div>
      </div>
      <div id="moon"></div>
    </label>
  </div>
</template>

<script>
export default {
  name: "DarkModeToggle",
  methods: {
    toggleTheme() {
      const body = document.querySelector("body");
      body.classList.toggle("dark-mode");
    },
  },
};
</script>

<style></style>
