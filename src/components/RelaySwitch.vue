<template>
  <div class="toggle-wrapper" @click="onClick">
    <div class="toggle-switch">
      <input
        type="checkbox"
        name="toggle-switch"
        :id="relay.id"
        class="toggle-switch"
        :checked="relay.state"
        @click="onClick"
      />
      <label class="toggle-body" for="toggle"></label>
    </div>
    <span class="toggle-label">{{ relay.name }}</span>
  </div>
</template>

<script>
export default {
  name: "RelaySwitch",
  props: {
    relay: {
      type: Object,
      required: true,
    },
  },
  methods: {
    onClick() {
      console.log(this.relay);
      this.$emit("toggleRelaySwitch", {
        state: !this.relay.state,
        id: this.relay.id,
      });
    },
  },
};
</script>

<style scoped>
.toggle-wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px 5px 5px 0px;
  margin: 5px;
  border: 2px solid var(--main-text-color);
  border-radius: 5px;
  transition: border-color 0.3s;
  width: 100%;
}

.toggle-switch {
  position: relative;
  display: flex;
  justify-content: space-evenly;
  width: 34px;
  height: 60px;
  margin-left: 8px;
  margin-right: 8px;
  vertical-align: middle;
}

.toggle-switch input {
  display: none;
  width: auto;
}

.toggle-body {
  display: inline-block;
  font-size: 14px;
  line-height: 60px;
  vertical-align: middle;
  color: var(--main-text-color);
  transition: color 0.3s;
  position: absolute;
  cursor: pointer;
  width: 100%;
  height: 100%;
  background-color: var(--toggle-bg-color);
  border-radius: 8px;
  transition: background-color 0.3s;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.toggle-label {
  display: flex;
  font-size: 14px;
  width: 75px;
  justify-content: center;
  vertical-align: middle;
  color: var(--main-text-color);
  transition: color 0.3s;
  text-align: center;
  user-select: none;
}

.toggle-body::before {
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

#toggle_checkbox {
  display: none;
}
</style>
