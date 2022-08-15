<template>
  <v-container>
    <v-col cols="12">
      <v-row id="board" @dragover.prevent @drop.prevent="drop" dense>
        <!-- <p>{{ this.shortcutData[2].title }}</p> -->
        <short-cut
          v-for="shortcut in shortcutData"
          :key="shortcut.id"
          :id="shortcut.id"
          :title="shortcut.title"
          @delete="deleteShortcut"
          @dragOn="draggableActivate"
          @dragOff="draggableDeactivate"
          :draggable="dragActivated"
        ></short-cut>
        <!-- <v-col class="addSC-col"> -->
        <v-card class="addSC-card" @click="addNewSC">
          <v-icon color="#EA6721"> mdi-plus </v-icon>
        </v-card>
        <!-- </v-col> -->
        <v-card
          class="waiting-card"
          v-for="n in squaredNumber"
          :key="n + 10"
        ></v-card>
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
import ShortcutCard from "./components/ShortcutCard.vue";

export default {
  name: "App",
  components: { shortCut: ShortcutCard },
  data() {
    return {
      shortcutData: [
        {
          id: 1,
          title: "Titre du module",
        },
        {
          id: 2,
          title: "TItre un petit peu long",
        },
        {
          id: 3,
          title:
            "Titre vraiment très long pour une si petite carte, ça fait beaucoup de mots",
        },
      ],
      newShortcutData: {
        id: Math.random(),
        title: "énième titre",
      },
      dragActivated: false,
      squaredNumber: null,
      new_id: null,
    };
  },
  computed: {
    calculateSquared() {
      console.log("Inner height :", window.innerHeight);
      console.log("DIVISE PAR 200 : ", window.innerHeight / 200);
      console.log(" ARRONDI : ", Math.ceil(window.innerHeight / 200));
      console.log(
        "Nombre de de carrés : ",
        (Math.ceil(window.innerHeight / 200) - 1) * 5 -
          this.shortcutData.length -
          1
      );
      let squaredNum =
        (Math.ceil(window.innerHeight / 200) - 1) * 5 -
        this.shortcutData.length -
        1;
      this.squaredNumber = squaredNum;
      return squaredNum;
    },
  },

  mounted() {
    this.calculateSquared;
    this.new_id = this.shortcutData.length;
  },
  methods: {
    deleteShortcut(scId) {
      console.log("DELETE SHORTCUT : ", scId);
      this.shortcutData = this.shortcutData.filter(
        (shortcut) => shortcut.id != scId
      );
      console.log("SQUARED NUMBER BEFORE DELETE : ", this.squaredNumber);
      this.squaredNumber = this.squaredNumber + 1;
    },
    addNewSC() {
      this.shortcutData.push(this.newShortcutData);
      this.squaredNumber = this.squaredNumber - 1;
    },
    drop(e) {
      const card_id = e.dataTransfer.getData("id");
      const card = document.getElementById(card_id);
      card.style.display = "block";
      const add_card = document.getElementsByClassName("addSC-card")[0];

      // e.target.appendChild(card);
      e.target.insertBefore(card, add_card);
    },
    draggableActivate() {
      console.log("DRAG ON");
      this.dragActivated = true;
    },
    draggableDeactivate() {
      console.log("DRAG OFF");
      this.dragActivated = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.addSC-card,
.waiting-card {
  width: 200px;
  height: 200px;
  border-radius: 10px;
  overflow: hidden;
  float: left;
  margin: 5px;

  .v-icon {
    top: 50%;
    left: 50%;
    transform: translateX(-50%) translateY(-50%);
    font-size: 80px;
    font-weight: 300;
  }
}
.waiting-card {
  background-color: #f5f7f8;
}
</style>
