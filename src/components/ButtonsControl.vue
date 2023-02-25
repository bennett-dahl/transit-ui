<template>
  <div>
    <p v-for="light in lights" :key="light.id">
      <button
        :class="{ pressed: light.state }"
        :id="light.id"
        :ref="light.id"
        v-on:click="toggle(light.id)"
        class="btn"
      >
        {{ light.name }}
      </button>
    </p>
    <!-- <input :id="val.id" v-on:click="log(val.id)" type="button" value= val.name> -->
  </div>
</template>

<script>
//import io from "socket.io-client";
export default {
  name: "ButtonsControl",
  props: { lights: Array },
  // data() {
  //   return {
  //     // socket: {},
  //     // lights: this.lights
  //   };
  // },

  methods: {
    toggle(data) {
      //this.socket.emit("move", direction);
      // console.log(document.getElementById(data));
      // console.log(this.$refs[data][0]);
      console.log(data);
      this.$emit("toggle", data);
      // this.$forceUpdate()
      //this.socket.emit("lightToggle", this.$refs[data][0].id);
    },
  },
  // watch: {
  //   _lights: function () {
  //     this.lights = this._lights;
  //   },
  // },

  // created() {
  //   this.socket = io("http://localhost:3000");
  // },
  // mounted() {
  //   this.socket.on("light", (data) => {
  //     this.lights = data;
  //     // console.log(this.lights);
  //   });
  //   this.socket.on("lightToggle", (data) => {
  //     console.log(this.lights)
  //     this.lights.forEach((light) => {
  //       if (light.id == data.id) {
  //         light.state = data.state;
  //       }
  //     });
  //     // this.lights = data;
  //     // this.$refs[data.id]
  //     // console.log(this.lights);
  //   });
  // },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
p {
  display: table-cell;
  padding-right: 5px;
}
.pressed {
  background-color: aqua;
}
</style>
