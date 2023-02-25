<template>
  <div class="container">
    <HeaderComp title="Transit Control" />
    <!-- <TasksAll
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    /> -->
    <ButtonsControl :lights="lights" @toggle="toggle" />
  </div>
</template>

<script>
import HeaderComp from "./components/HeaderComp";
//import TasksAll from "./components/TasksAll.vue";
import "@fortawesome/fontawesome-free/js/all.js";
import "bootstrap";
import ButtonsControl from "./components/ButtonsControl.vue";

export default {
  name: "App",
  components: {
    HeaderComp,
    ButtonsControl,
  },
  data() {
    return {
      socket: require("socket.io-client")("http://192.168.50.13:3000"),
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
    toggle(val) {
      this.socket.emit("lightToggle", val);
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
      this.lights = data.lights;
      console.log(this.lights);
      //this.power_data = data.power_data;
    });
    this.socket.on("lightToggle", (data) => {
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
* {
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
}
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
