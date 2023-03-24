<template>
  <div class="app">
    <HeaderComp @toggleOverhead="toggleOverhead" title="Transit Control" />
    <div class="home-container">
      <!-- <ButtonsControl :relays="relays" @toggle="toggle" /> -->
      <div class="controls">
        <RelaysControl
          :relays="relays"
          @toggleRelaySwitch="toggleRelaySwitch"
        />
        <LightsControl
          :lights="lights"
          @toggleLightSwitch="toggleLightSwitch"
        />
      </div>
    </div>
  </div>
</template>

<script>
import HeaderComp from "./components/HeaderComp";
import "@fortawesome/fontawesome-free/js/all.js";
import "bootstrap";
//import ButtonsControl from "./components/ButtonsControl.vue";
import LightsControl from "./components/LightsControl.vue";
import RelaysControl from "./components/RelaysControl.vue";

export default {
  name: "App",
  components: {
    HeaderComp,
    //ButtonsControl,
    LightsControl,
    RelaysControl,
  },
  data() {
    return {
      socket: require("socket.io-client")("http://192.168.50.13:3000"),
      relays: [],
      lights: [],
    };
  },

  methods: {
    //these functions are for outgoing data to the server
    toggle(buttonID) {
      this.socket.emit("relayToggle", buttonID);
      console.log(buttonID);
    },
    //this function toggles the overhead light by emitting the inverse of the ui state of the light
    toggleOverhead(buttonID) {
      console.log(buttonID);
      console.log("emitting overheadLights from Client");
      this.socket.emit(
        "overheadLights",
        this.lights.filter((el) => el.id === buttonID).state
      );
    },
    toggleLightSwitch(data) {
      this.lights = this.lights.map((el) => {
        return el.id == data.id ? { ...el, state: data.state } : el;
      });
      console.log("emitting lightToggle from Client");
      this.socket.emit("overheadLights", data);
    },
    toggleRelaySwitch(data) {
      this.relays = this.relays.map((el) => {
        return el.id == data.id ? { ...el, state: data.state } : el;
      });
      console.log("emitting relayToggle from Client");
      this.socket.emit("relayToggle", data);
    },
  },
  created() {
    //these functions are for incoming data from the server

    //this function initializes the ui state of the relays and lights
    this.socket.on("init", (data) => {
      this.response = data;
      this.relays = [...data.relays];
      this.lights = [...data.lights];
      console.log(this.relays);
      console.log(this.lights);
    });
    this.socket.on("relayToggle", (data) => {
      this.relays.forEach((el) => {
        if (el.id == data.id) {
          el.state = data.state;
        }
      });
    });
    this.socket.on("toggleOverhead", (data) => {
      this.lights.forEach((el) => {
        if (el.id == data.id) {
          el.state = data.state;
        }
      });
    });
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");

:root {
  --main-bg-color: #ffffff;
  --main-text-color: #000000;
  --button-bg-color: #007bff;
  --button-text-color: #ffffff;
  --toggle-bg-color: #ccc;
  --toggle-switch-color: #fff;
  --indicator-light-off: #888;
  --indicator-light-on: #0f0;
}

body {
  font-family: Arial, sans-serif;
  background-color: var(--main-bg-color);
  color: var(--main-text-color);
  margin: 0;
  padding: 0;
}
.home-container {
  display: flex;
  flex: wrap;
  align-items: center;
  justify-content: space-between;
  margin: 0 auto;
  width: 90%;
  max-width: 800px;
  padding: 20px;
}
.container {
  width: 100%;
  margin: 0 auto;
  text-align: center;
  display: flex;
}

.controls {
  display: flex;
  width: 100%;
  justify-content: space-between;
}

.control-button {
  background-color: var(--button-bg-color);
  color: var(--button-text-color);
  padding: 10px 20px;
  /* margin: 20px; */
  border: none;
  cursor: pointer;
  border-radius: 8px;
  transition: background-color 0.3s, box-shadow 0.2s;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  vertical-align: center;
}

.control-button:hover {
  background-color: #0069d9;
}

.control-button:active {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transform: translateY(2px);
}

nav {
  display: flex;
  justify-content: space-between;
  background-color: var(--toggle-bg-color);
  margin-bottom: 10px;
}

nav .title {
  font-size: 2em;
  margin: 15px;
}

.theme-toggle {
  /* background-color: var(--button-bg-color); */
  /* color: var(--button-text-color); */
  display: flex;
  position: relative;
  width: 116px;
  /* padding: 10px 20px; */
  margin: 33px 0;
  border: none;
  /* cursor: pointer; */
  /* transition: background-color 0.3s; */
}

.theme-toggle label {
  display: block;
  position: absolute;
  top: 10;
  right: 0;
  left: 10;
  width: 116px;
  height: 56px;
  margin: 8px auto;
  background-color: #77b5fe;
  border-radius: 56px;
  transform: translateY(-50%);
  cursor: pointer;
  transition: 0.3s ease background-color;
  overflow: hidden;
}

#star {
  position: absolute;
  top: 13px;
  left: 13px;
  width: 30px;
  height: 30px;
  background-color: #fafd0f;
  transform: scale(1);
  border-radius: 50%;
  transition: 0.3s ease top, 0.3s ease left, 0.3s ease transform,
    0.3s ease background-color;
  z-index: 1;
}

#star-1 {
  position: relative;
}

#star-2 {
  position: absolute;
  transform: rotateZ(36deg);
}

.star {
  top: 0;
  left: -7px;
  font-size: 54px;
  line-height: 28px;
  color: #fafd0f;
  transition: 0.3s ease color;
}

#moon {
  position: absolute;
  bottom: -52px;
  right: 8px;
  width: 40px;
  height: 40px;
  background-color: #fff;
  border-radius: 50%;
  transition: 0.3s ease bottom;
}

#moon:before {
  content: "";
  position: absolute;
  top: -12px;
  left: -17px;
  width: 40px;
  height: 40px;
  background-color: #03a9f4;
  border-radius: 50%;
  transition: 0.3s ease background-color;
}

#toggle_checkbox:checked + label {
  background-color: #000;
}

#toggle_checkbox:checked + label #star {
  top: 3px;
  left: 64px;
  transform: scale(0.3);
  background-color: yellow;
}

#toggle_checkbox:checked + label .star {
  color: yellow;
}

#toggle_checkbox:checked + label #moon {
  bottom: 8px;
}

#toggle_checkbox:checked + label #moon:before {
  background-color: #000;
}

body.dark-mode {
  --main-bg-color: #222222;
  --main-text-color: #f1f1f1;
  --button-bg-color: #495057;
  --button-text-color: #ffffff;
  --toggle-bg-color: #999;
  --toggle-switch-color: #333;
  --indicator-light-off: #555;
  --indicator-light-on: #0f0;
}

body.dark-mode .toggle-label {
  color: var(--main-text-color);
}

body.dark-mode .toggle-wrapper {
  border-color: var(--main-text-color);
}

/* ... Your existing CSS styles ... */

@media (max-width: 600px) {
  /* Styles for mobile devices */

  .controls {
    align-items: center;
    margin-bottom: 15px;
  }

  .control-button {
    padding: 8px 15px;
    font-size: 14px;
  }

  .theme-toggle {
    margin: 25px 5px;
  }
}

@media (min-width: 601px) and (max-width: 1024px) {
  /* Styles for tablet devices */

  .theme-toggle {
    margin: 25px 5px;
  }

  nav .title {
    font-size: 1.8em;
    margin: 12px;
  }
}

@media (min-width: 1025px) {
  /* Styles for desktop devices */

  /* .control-button {
    padding: 10px 20px;
    font-size: 18px;
  }

  .toggle-wrapper {
    padding: 5px 5px 5px 0px;
  }

  .toggle-label {
    font-size: 14px;
    line-height: 60px;
  } */

  .theme-toggle {
    margin: 33px 12px;
  }

  nav .title {
    font-size: 2em;
    margin: 15px;
  }
}

/* * {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
} */
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
