<template>
  <div class="datapreview">
        <vs-table :data="floorData">
      <template slot="thead">
        <vs-th>
          Floor
        </vs-th>
        <vs-th>
          Room
        </vs-th>
        <vs-th>
          Equipement
        </vs-th>
      </template>

      <template slot-scope="{data}">
        <vs-tr :key="indextr" v-for="(tr, indextr) in data" >
          <vs-td :data="data[indextr].Floor">
            {{data[indextr].Floor}}
          </vs-td>

          <vs-td :data="data[indextr].Room">
            {{data[indextr].Room}}
          </vs-td>

          <vs-td :data="data[indextr].Equipement">
            {{data[indextr].Equipement}}
          </vs-td>
        </vs-tr>
      </template>
    </vs-table>
  </div>
</template>

<script>
import { EventBus } from "../../config/event";

export default {
  name: "previewdata",
  props: ["allData", "floorSelected"],
  data() {
    return {
      data: [],
      floorData: []
    }
  },
  methods: {
    update() {
      this.floorData = [];
      let self = this;
      let arr = this.data.rooms;

      for (var iterator in arr)
          self.extractPreviewInfo(arr[iterator], iterator);
    },
    selectFloor(int) {
      this.floorData = [];
      let self = this;
      let arr = this.data;

      for (var iterator in arr)
        for (var floors in arr[iterator])
          if (floors === int)
            self.extractPreviewInfo(arr[iterator][floors], floors);
    },
    extractPreviewInfo(arr, int) {
      let equipment;
      let self = this;

      for (var rooms in arr)
        for (var index in arr[rooms])
          if (typeof arr[rooms][index].id !== "undefined") {
            equipment = self.getLength(arr[rooms][index].equipements);
            self.floorData.push({ Floor: self.data.floors[int].name,
                Room: arr[rooms][index].name,
                Equipement: equipment });
            }
    },
    getLength(value) {
      if (value === undefined)
        return 0;
      return value.length;
    },
    getEvents() {
     EventBus.$on("click-event", (it) => {
      for (var iterator in this.data) {
        for (var floor in this.data[iterator])
            if (this.data[iterator][floor].id === it.id)
              this.selectFloor(floor);
      }
     });
    }
  },
  mounted() {
    this.data = this.allData;
    this.getEvents();
  },
   watch: {
    data: function() {
      if (this.floorSelected != "") {
          for (var ite in this.data.floors)
            if (this.data.floors[ite].name == this.floorSelected)
              this.selectFloor(ite);
        }
        else
          this.update();
    },
    floorSelected: function() {

    }
  }
};
</script>

<style scoped>

</style>

