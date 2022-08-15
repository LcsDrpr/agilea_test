<template>
  <!-- <v-col> -->
  <v-card
    data-app
    :id="id"
    :draggable="draggable"
    @dragstart="dragStart"
    @dragover.stop
  >
    <v-btn
      @mousedown="switchDragOn"
      @mouseleave="switchDragOff"
      elevation="0"
      color="#FFFFFF"
      class="drag-btn"
    >
      <v-icon color="#EA6721">mdi-drag-vertical</v-icon></v-btn
    >
    <v-img
      src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
      height="60px"
    ></v-img>
    <v-avatar color="#FDE3CE" size="36">
      <v-icon dark color="#EA6721"> mdi-school-outline </v-icon>
    </v-avatar>
    <v-card-text class="text--primary">
      <p>{{ title }}</p>
    </v-card-text>
    <v-dialog v-model="dialog">
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          v-bind="attrs"
          v-on="on"
          fab
          x-small
          elevation="0"
          color="#FFFFFF"
          class="delete-btn"
        >
          <v-icon dark color="#EA6721">mdi-trash-can-outline</v-icon>
        </v-btn>
      </template>
      <v-card>
        <v-card-text>
          Êtes-vous sûr de vouloir supprimer ce raccourcis ?
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn @click="deleteSC">Oui</v-btn>
          <v-btn color="primary" text @click="dialog = false"> Non </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-card>
  <!-- </v-col> -->
</template>
<script>
export default {
  props: ["id", "title", "draggable"],
  emits: ["delete", "dragOn", "dragOff"],
  data() {
    return {
      dialog: false,
    };
  },

  methods: {
    switchDragOn() {
      this.$emit("dragOn");
    },
    switchDragOff() {
      this.$emit("dragOff");
    },
    deleteSC() {
      console.log("CLICK DELETESC");
      this.$emit("delete", this.id);
      this.dialog = !this.dialog;
    },
    dragStart(e) {
      const target = e.target;
      e.dataTransfer.setData("id", target.id);

      setTimeout(() => {
        target.style.display = "none";
      }, 0);
    },
  },
};
</script>

<style lang="scss" scoped>
.v-card {
  width: 200px;
  height: 200px;
  border-radius: 10px;
  overflow: hidden;
  float: left;
  margin: 5px;

  .drag-btn {
    position: absolute;
    top: -2px;
    left: -9px;
    z-index: 3000;
    width: 20px;
    border-bottom-right-radius: 15px;
    height: 25px;
    min-width: auto;

    i {
      font-size: 20px;
      position: absolute;
      right: -12px;
    }
  }

  .delete-btn {
    position: absolute;
    top: 1px;
    right: 2px;
    width: 21px;
    height: 21px;
    i {
      font-size: 15px;
    }
  }
  .v-avatar {
    position: absolute;
    top: 30px;
    left: 20px;
  }
  .v-card__text {
    height: calc(100% - 60px);
    display: flex;
    justify-content: center;
    align-items: center;

    p {
      color: #0c5983;
      font-family: "Roboto";
      text-align: center;
    }
  }
}
</style>
