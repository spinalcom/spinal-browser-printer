<template>
  <div class="middleBar">

    <md-icon class="material-icons custom-icon" value="room" v-on:click.native="onClick">room</md-icon>
    <div class="display-icon"></div>
    <md-icon class="material-icons custom-icon" value="device" v-on:click.native="onClick">devices</md-icon>

  </div>
</template>

<script>
import { EventBus } from "../../config/event";

export default {
  name: "MiddleBar",
  data() {
    return {
      data: []
    }
  },
  // mounted() {
  //   this.getEvents();
  // },
  methods: {
    onClick() {

      let choice = event.target.getAttribute("value");

      if (choice === "room")
        EventBus.$emit("choose-room");
      else if (choice === "device")
        EventBus.$emit("choose-device");

    }
  }
};
</script>

<style scoped>
.middleBar {
  width: 100%;
  border: solid;
  border-width: 1px;
  margin-right: 1%;
  margin-left: 4px;
  height: 6%;
  overflow: auto;
}
.material-icons {
  font-size: 230% !important;
  margin-top: 0.4%;  
}

.display-icon {
  padding-right: 2px;
  display: inline-block;
}

.custom-icon {
  color: black;
}


</style>
