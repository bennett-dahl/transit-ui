<template>
  <div class="toggle-wrapper">
    <div class="toggle-switch">
      <input
        type="checkbox"
        name="toggle-switch"
        :id="light.id"
        class="toggle-switch"
        :checked="light.state"
        @click="onClick"
      />
      <label class="toggle-label" for="toggle" @click="onClick"></label>
    </div>
    <span class="toggle-label">{{ light.name }}</span>
  </div>

  <!-- <button @click="onClick()" :style="{ background: color }" class="btn">
      {{ text }}
    </button> -->
</template>

<script>
export default {
  name: "LightSwitch",
  props: {
    light: {
      type: Object,
      required: true,
    },
    // text: String,
    // id: Number,
    // state: Boolean,
  },
  methods: {
    onClick() {
      console.log(this.light);
      this.$emit("toggleLightSwitch", {
        state: !this.light.state,
        id: this.light.id,
      });
      //   console.log("Clicked LightSwitch");
      //   console.log({ state: this.state, id: this.id });
    },
  },
};
</script>
