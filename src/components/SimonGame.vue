<template>
  <main>
    <loosing-modal v-if="loosing" @close="restart" :round="round" />

    <div class="simon-info">
      <h1>SIMON SAYS</h1>
      <h2>Уровень сложности: {{ сomplexity }}</h2>
      <button class="start-btn" @click="startSimon">Начать</button>

      <div class="simon-сomplexity">
        <button class="сomplexity-btn easy" @click="сomplexity = 'Легкий'">
          Легкая
        </button>
        <button class="сomplexity-btn mediun" @click="сomplexity = 'Средний'">
          Средняя
        </button>
        <button class="сomplexity-btn hard" @click="сomplexity = 'Сложный'">
          Сложная
        </button>
      </div>
    </div>

    <div class="simon-template">
      <div class="panel-top">
        <div
          class="blue-panel panel"
          id="blue-panel"
          @click="targetClick($event.currentTarget)"
        ></div>
        <div
          class="red-panel panel"
          id="red-panel"
          @click="targetClick($event.currentTarget)"
        ></div>
      </div>
      <div class="panel-bottom">
        <div
          class="yellow-panel panel"
          id="yellow-panel"
          @click="targetClick($event.currentTarget)"
        ></div>
        <div
          class="green-panel panel"
          id="green-panel"
          @click="targetClick($event.currentTarget)"
        ></div>
      </div>
    </div>
  </main>
</template>

<script>
import LoosingModal from "./UI/LoosingModal.vue";

export default {
  data: function () {
    return {
      сomplexity: "Легкий",
      round: 1,
      click: false,
      randomPanel: [],
      guessPanel: [],
      loosing: false,
    };
  },
  components: {
    LoosingModal,
  },
  methods: {
    startSimon() {
      this.randomPanel.push(this.getRandomPanel());
      this.guessPanel = [...this.randomPanel];

      const start = async () => {
        this.click = true;

        for (let panel of this.randomPanel) {
          await this.flashPanel(panel);
        }
      };
      start();
    },
    getRandomPanel() {
      const green = document.querySelector(".green-panel");
      const blue = document.querySelector(".blue-panel");
      const red = document.querySelector(".red-panel");
      const yellow = document.querySelector(".yellow-panel");

      const panels = [green, blue, red, yellow];
      return panels[parseInt(Math.random() * panels.length)];
    },
    flashPanel(panel) {
      const time =
        this.сomplexity === "Легкий"
          ? 1500
          : this.сomplexity === "Средний"
          ? 1000
          : 500;

      return new Promise((resolve) => {
        panel.className += " target";

        this.audioOnClick(panel.id);
        setTimeout(() => {
          panel.className = panel.className.replace(" target", "");

          setTimeout(() => {
            resolve();
          }, 500);
        }, time);
      });
    },
    targetClick(target) {
      if (!this.click) return;

      this.audioOnClick(target.id);
      const expectedPanel = this.guessPanel.shift();

      if (expectedPanel === target) {
        if (this.guessPanel.length === 0) {
          this.guessPanel = [...this.randomPanel];
          this.round += 1;

          setTimeout(() => {
            this.startSimon();
          }, 1000);
        }
      } else {
        this.loosing = true;
        this.randomPanel = [];
        this.guessPanel = [];
        this.click = false;
      }
    },
    audioOnClick(color) {
      let audio;
      switch (color) {
        case "blue-panel":
          audio = new Audio(
            "https://s3.amazonaws.com/freecodecamp/simonSound1.mp3"
          );
          audio.play();
          break;
        case "red-panel":
          audio = new Audio(
            "https://s3.amazonaws.com/freecodecamp/simonSound2.mp3"
          );
          audio.play();
          break;
        case "yellow-panel":
          audio = new Audio(
            "https://s3.amazonaws.com/freecodecamp/simonSound3.mp3"
          );
          audio.play();
          break;
        case "green-panel":
          audio = new Audio(
            "https://s3.amazonaws.com/freecodecamp/simonSound4.mp3"
          );
          audio.play();
          break;
      }
    },
    restart() {
      this.loosing = false;
      this.round = 1;
    },
  },
};
</script>

<style scoped>
@import url("../styles/components/SimonGame.css");
</style>
