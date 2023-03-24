<template>
  <div class="relays-control">
    <h1>Relays</h1>
    <div class="relays">
      <div class="relay" v-for="relay in relays" :key="relay.id">
        <RelaySwitch :relay="relay" @toggleRelaySwitch="toggleRelaySwitch" />
      </div>
    </div>
  </div>
</template>

<script>
import RelaySwitch from "./RelaySwitch.vue";
export default {
  name: "RelaysControl",
  components: {
    RelaySwitch,
  },
  props: {
    relays: { type: Array, required: true },
  },
  methods: {
    toggleRelaySwitch(data) {
      this.$emit("toggleRelaySwitch", data);
    },
  },
};
</script>

<style scoped>
h1 {
  text-align: center;
}
.relay {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}
</style>
