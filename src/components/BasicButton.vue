<template>
  <!-- <div class="toggle-wrapper">
    <div class="toggle-switch">
      <input
        type="checkbox"
        name="toggle-switch"
        id="toggle"
        class="toggle-switch-checkbox"
        :checked="checked"
        @change="onClick"
      />
      <label class="toggle-label" for="toggle"></label>
    </div>
    <span class="toggle-label">{{ text }}</span>
  </div> -->

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

<style scoped>
.toggle-wrapper {
  display: inline-flex;
  align-items: center;
  padding: 5px 5px 5px 0px;
  border: 2px solid var(--main-text-color);
  border-radius: 5px;
  transition: border-color 0.3s;
}

.toggle-switch {
  position: relative;
  display: flex;
  width: 34px;
  height: 60px;
  vertical-align: middle;
  margin-left: 10px;
}

.toggle-switch input {
  display: none;
}

.toggle-switch label {
  position: absolute;
  cursor: pointer;
  width: 100%;
  height: 100%;
  background-color: var(--toggle-bg-color);
  border-radius: 8px;
  transition: background-color 0.3s;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  margin-right: 10px;
}

.toggle-switch label::before {
  position: absolute;
  content: "";
  width: 24px;
  height: 24px;
  background-color: var(--toggle-switch-color);
  border-radius: 25%;
  left: 3px;
  top: 28px;
  transition: top 0.3s, background-color 0.3s;
  border: 2px solid #222222; /* Add border to the inside sliding part */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2); /* Add drop-shadow to the inside sliding part */
}

.toggle-switch input:checked + label::before {
  top: 4px;
  background-color: var(
    --indicator-light-on
  ); /* Change the color of the sliding part */
}

.toggle-label {
  display: inline-block;
  font-size: 14px;
  margin-left: 10px;
  line-height: 60px;
  vertical-align: middle;
  color: var(--main-text-color);
  transition: color 0.3s;
}
</style>
