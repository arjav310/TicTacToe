<template>
  <main
    class="bg-dark text-light d-flex flex-column justify-content-center align-items-center"
  >
    <div class="container">
      <div class="row justify-content-center">
        <div class="col col-6">
          <div class="text-center">
            <h1 class="mb-4">Let's Play!</h1>
            <div v-if="!winMsg">
              <h3 v-show="isCross">Cross Turn</h3>
              <h3 v-show="!isCross">Circle Turn</h3>
            </div>
            <div v-else>
              <h3 class="text-warning">{{ winMsg.toUpperCase() }}</h3>
            </div>
          </div>
          <div class="grid mt-3">
            <div
              class="card card-body box justify-content-center align-items-center bg-light"
              v-for="(item, i) in itemArray"
              :key="i"
              @click="handleClick(i)"
            >
              <GameIcon :iconName="item"></GameIcon>
            </div>
          </div>
          <div class="text-center mt-3">
            <button class="btn btn-warning" @click="resetGame">Reset</button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import GameIcon from "./components/GameIcon.vue";
import Swal from "sweetalert2/dist/sweetalert2.js";
import { nextTick } from "vue";

export default {
  name: "App",
  components: {
    GameIcon,
  },
  data() {
    return {
      winMsg: "",
      isCross: true,
      itemArray: new Array(9).fill("empty"),
    };
  },
  watch: {
    winMsg: function (msg) {
      if (msg) {
        setTimeout(() => {
          this.showDialog();
        }, 500);
      }
    },
  },
  methods: {
    showDialog() {
      Swal.fire({
        icon: "info",
        title: `${this.winMsg}`.toUpperCase(),
      });
      nextTick(() => {
        setTimeout(() => {
          this.resetGame();
        }, 2000);
      });
    },
    handleClick(i) {
      if (this.winMsg) {
        return this.showDialog();
      }

      if (this.itemArray[i] === "empty") {
        this.itemArray[i] = this.isCross ? "cross" : "circle";
        this.isCross = !this.isCross;
      } else {
        return Swal.fire("This position is already filled");
      }

      this.checkWinner();
    },
    async checkWinner() {
      const winCombo = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];
      for (let i = 0; i < winCombo.length; i++) {
        const [a, b, c] = winCombo[i];
        if (
          this.itemArray[a] !== "empty" &&
          this.itemArray[a] === this.itemArray[b] &&
          this.itemArray[b] === this.itemArray[c]
        ) {
          this.winMsg = `${this.itemArray[a]} won`;
          return false;
        }
      }

      if (this.itemArray.filter((item) => item === "empty").length === 0) {
        this.winMsg = "Game Draw";
        return false;
      }

      return false;
    },
    resetGame() {
      this.winMsg = "";
      this.isCross = true;
      this.itemArray = new Array(9).fill("empty");
    },
  },
};
</script>

<style>
main {
  height: 100vh;
}
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 10px;
}

.box {
  height: 150px;
}

button {
  width: 100px;
}

@media screen and (max-width: 600px) {
  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 5px;
    justify-content: center;
    align-items: center;
  }
  .box {
    height: 80px;
    width: 80px;
  }
}

@media screen and (max-width: 900px) {
  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 10px;
    justify-content: center;
    align-items: center;
  }
  .box {
    height: 100px;
  }
}
</style>
