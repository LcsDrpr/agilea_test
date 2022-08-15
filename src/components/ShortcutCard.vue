<template>
  <!-- <v-col> -->

  <v-card
    data-app
    :id="id"
    :draggable="draggable"
    @dragstart="dragStart"
    @dragover.stop
    :class="[cardSizeXS ? 'col-5' : '']"
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
      height="55px"
    ></v-img>
    <v-avatar
      color="#FDE3CE"
      size="36"
      :class="[cardSizeXS ? 'avatar-xs' : '']"
    >
      <v-icon dark color="#EA6721"> mdi-school-outline </v-icon>
    </v-avatar>
    <v-card-text class="text--primary" :class="[cardSizeXS ? 'text-xs' : '']">
      <p>{{ title }}</p>
    </v-card-text>
    <v-dialog v-model="dialog" max-width="400">
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
      <v-card width="390" class="deleteSC-container">
        <v-card-text>
          Êtes-vous sûr de vouloir supprimer ce raccourcis ?
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="#4D6E80"
            text
            @click="dialog = false"
            width="80px"
            plain
            class="no-btn"
          >
            Non
          </v-btn>
          <v-btn
            @click="deleteSC"
            color="#EA6721"
            width="80px"
            plain
            class="yes-btn"
            >Oui</v-btn
          >
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
      cardSizeXS: false,
    };
  },
  computed: {
    checkWindowWidth() {
      const screenWidth = window.innerWidth;
      if (screenWidth < 600) {
        this.cardSizeXS = true;
      }
    },
  },
  methods: {
    switchDragOn() {
      this.$emit("dragOn");
    },
    switchDragOff() {
      this.$emit("dragOff");
    },
    deleteSC() {
      console.log("DELETE SCID COMPONENT : ", this.id);
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
  mounted() {
    this.checkWindowWidth;
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
    top: 20%;
    left: 20px;
    transform: translateY(-20%);
    &.avatar-xs {
      top: 23%;
      transform: translateY(-23%);
    }
  }
  .v-card__text {
    height: calc(100% - 55px);
    display: flex;
    justify-content: center;
    align-items: center;

    p {
      color: #0c5983;
      font-family: "Roboto";
      text-align: center;
    }
    &.text-xs {
      font-size: 12px;
    }
  }

  .deleteSC-container {
    height: 150px;
    width: 390px;
    font-family: "Roboto";

    .v-card__text {
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 0;
      color: #0c5983;
    }
  }
}
</style>
