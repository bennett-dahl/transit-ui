<template>
  <div class="app">
    <HeaderComp @toggleOverhead="toggleOverhead" title="Transit Control" />
    <div class="home-container">
      <!-- <TasksAll
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    /> -->
      <ButtonsControl :relays="relays" @toggle="toggle" />
      <LightsControl :lights="lights" @toggleLightSwitch="toggleLightSwitch" />
    </div>
  </div>
</template>

<script>
import HeaderComp from "./components/HeaderComp";
//import TasksAll from "./components/TasksAll.vue";
import "@fortawesome/fontawesome-free/js/all.js";
import "bootstrap";
import ButtonsControl from "./components/ButtonsControl.vue";
import LightsControl from "./components/LightsControl.vue";

export default {
  name: "App",
  components: {
    HeaderComp,
    ButtonsControl,
    LightsControl,
  },
  data() {
    return {
      socket: require("socket.io-client")("http://192.168.50.13:3000"),
      relays: [],
      lights: [],
      tasks: [],
    };
  },

  methods: {
    deleteTask(id) {
      if (
        confirm(
          "Are you sure you'd like to delete task: " +
            this.tasks.filter((task) => task.id == id)[0].text
        )
      ) {
        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },
    toggleReminder(task) {
      // console.log("app toggle reminder " + JSON.stringify(id));

      console.log(this.tasks[task.id - 1]);

      this.tasks[task.id - 1] = {
        ...task,
        reminder: this.tasks[task.id - 1] == true ? false : true,
      };

      console.log(this.tasks[task.id - 1]);
      // this.task[task.id - 1] =
      // this.tasks = this.tasks.map((t) => {
      //   t.id === task.id ? { ...t, reminder: !task.reminder } : t;
      // });
    },
    toggle(buttonID) {
      this.socket.emit("relayToggle", buttonID);
      console.log(buttonID);
    },
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
      // console.log(this.lights);
      console.log("emitting lightToggle from Client");
      this.socket.emit("overheadLights", data);
    },
  },
  created() {
    this.tasks = [
      {
        id: 1,
        text: "Doctors Appointment",
        day: "March 1st",
        reminder: true,
      },
      {
        id: 2,
        text: "Meeting",
        day: "March 2nd",
        reminder: true,
      },
      {
        id: 3,
        text: "Grocery Shopping",
        day: "March 5th",
        reminder: false,
      },
    ];
    this.socket.on("init", (data) => {
      this.response = data;
      this.relays = [...data.relays];
      this.lights = [...data.lights];
      console.log(this.relays);
      console.log(this.lights);
      //this.power_data = data.power_data;
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
  display: flexbox;
  flex-direction: fl;
  align-items: center;
  justify-content: space-between;
  margin: 0 auto;
  width: 100%;
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

#toggle_checkbox {
  display: none;
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
  margin: 0 auto;
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
