<template>
  <v-container>
    <v-col cols="12">
      <v-row id="board" @dragover.prevent @drop.prevent="drop" dense>
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
        <v-card
          class="addSC-card"
          @click="addNewSC"
          :class="[cardSizeXS ? 'col-5' : '']"
        >
          <v-icon color="#EA6721"> mdi-plus </v-icon>
        </v-card>
        <v-card
          class="waiting-card"
          :class="[cardSizeXS ? 'col-5' : '']"
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
      dragActivated: false,
      squaredNumber: null,
      new_id: null,
      cardSizeXS: false,
    };
  },
  computed: {
    // Fonction qui donne la largeur de l'écran afin d'activer une partie du responsive.
    checkWindowWidth() {
      const screenWidth = window.innerWidth;
      if (screenWidth < 600) {
        this.cardSizeXS = true;
      } else {
        this.cardSizeXS = false;
      }
      return screenWidth;
    },
  },

  mounted() {
    this.calculateSquared();
    this.new_id = this.shortcutData.length;
    this.checkWindowWidth;
  },
  methods: {
    //Fonction qui supprimer un raccourcis, réorganise les éléments en fonction des raccourcis restant et remplace à l'affichage un raccourcis que l'on vient d'enlever par un élément grisé.
    deleteShortcut(scId) {
      this.shortcutData = this.shortcutData.filter(
        (shortcut) => shortcut.id != scId
      );
      this.squaredNumber = this.squaredNumber + 1;
    },

    //Fonction qui permet d'ajouter un nouveau raccourcis et de supprimer un élément grisé.
    addNewSC() {
      let randomId = Math.random();
      this.shortcutData.push({
        id: randomId,
        title: "énième titre",
      });
      this.squaredNumber = this.squaredNumber - 1;
    },

    //Fonction qui permet de faire le changement d'emplacement du raccourcis au drop.
    drop(e) {
      const card_id = e.dataTransfer.getData("id");
      const card = document.getElementById(card_id);
      card.style.display = "block";
      const add_card = document.getElementsByClassName("addSC-card")[0];

      // e.target.appendChild(card);
      e.target.insertBefore(card, add_card);
    },

    //Fonction qui active le drag & drop lorsque l'on attrape l'icone
    draggableActivate() {
      this.dragActivated = true;
    },
    //Fonction qui désactive le drag & drop lorsque l'on lâche l'icone
    draggableDeactivate() {
      this.dragActivated = false;
    },

    // Fonction qui permet de calculer le nombre d'éléments grisés à ajouter en fonction de la taille de l'écran.
    calculateSquared() {
      let squaredNum =
        (Math.ceil(window.innerHeight / 200) - 1) * 5 -
        this.shortcutData.length -
        1;
      this.squaredNumber = squaredNum;
      return squaredNum;
    },
  },
};
</script>

<style lang="scss" scoped>
#board {
  justify-content: center;
}
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
