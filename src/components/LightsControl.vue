<template>
  <div class="lights-control">
    <h1>Lights</h1>
    <div class="lights">
      <div class="light" v-for="light in lights" :key="light.id">
        <LightSwitch :light="light" @toggleLightSwitch="toggleLightSwitch" />
      </div>
    </div>
  </div>
</template>

<script>
import LightSwitch from "./LightSwitch.vue";
export default {
  name: "LightsControl",
  components: {
    LightSwitch,
  },
  props: {
    lights: { type: Array, required: true },
  },
  methods: {
    toggleLightSwitch(data) {
      this.$emit("toggleLightSwitch", data);
      //   console.log("LightsControl");
      //   console.log(data);
    },
  },
};
</script>

<style></style>
<!-- :text="light.name"
          :id="light.id"
          :checked="light.state" -->
