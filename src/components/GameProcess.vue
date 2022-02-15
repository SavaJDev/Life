<template>
  <v-container>
    <v-row class="justify-center mt-1 mb-1">
      <play-field
        :sizex="sizex"
        :sizey="sizey"
        :initArr.sync="initArr"
        :gameLaunched="gameLaunched"
      ></play-field>
    </v-row>
    <v-row class="justify-center">
      <v-btn
        class="mr-6"
        color="green"
        :disabled="blockBtn || blocked"
        @click="startGame"
        >Start</v-btn
      >
      <v-btn class="mr-6" color="red" @click="stopGame">Stop</v-btn>
      <v-btn :disabled="blockBtn || blocked" @click="clearPlayField"
        >Clear</v-btn
      >
    </v-row>
  </v-container>
</template>

<script>
import PlayField from "./PlayField";

export default {
  name: "GameProcess",

  components: {
    PlayField,
  },
  data: () => ({
    sizex: 30,
    sizey: 30,
    initArr: [],
    timerId: null,
    periodTime: 100,
    blocked: false,
    gameLaunched: false,
  }),
  methods: {
    // Запуск игры
    startGame() {
      if (this.initArr.length !== 0) {
        this.gameLaunched = true;
        this.blocked = true;
        this.timerId = setInterval(() => {
          this.periodСalc();
          if (this.activeCellCount == 0) this.stopGame();
        }, this.periodTime);
      }
    },

    // Остановка игры
    stopGame() {
      clearInterval(this.timerId);
      this.gameLaunched = false;
      this.blocked = false;
    },

    // Логика игры
    periodСalc() {
      let temp = [];
      let xl,
        xr,
        yt,
        yb,
        q = null;

      for (let y = 0; y < this.sizey; y++) {
        yt = y - 1;
        if (yt == -1) yt = this.sizey - 1;
        yb = y + 1;
        if (yb == this.sizey) yb = 0;
        temp[y] = [];
        for (let x = 0; x < this.sizex; x++) {
          xl = x - 1;
          if (xl == -1) xl = this.sizex - 1;
          xr = x + 1;
          if (xr == this.sizex) xr = 0;
          q =
            this.initArr[yt][xl].alive +
            this.initArr[yt][x].alive +
            this.initArr[yt][xr].alive +
            this.initArr[y][xl].alive +
            this.initArr[y][xr].alive +
            this.initArr[yb][xl].alive +
            this.initArr[yb][x].alive +
            this.initArr[yb][xr].alive;

          if (this.initArr[y][x].alive == 0 && q == 3) {
            temp[y][x] = { alive: 1 };
          } else if (this.initArr[y][x].alive == 1 && (q == 2 || q == 3)) {
            temp[y][x] = { alive: 1 };
          } else {
            temp[y][x] = { alive: 0 };
          }
        }
      }
      for (let y = 0; y < this.sizey; y++) {
        for (let x = 0; x < this.sizex; x++) {
          this.initArr[y][x].alive = temp[y][x].alive;
        }
      }
    },

    // Очистка игрового поля
    clearPlayField() {
      for (let y = 0; y < this.sizey; y++) {
        for (let x = 0; x < this.sizex; x++) {
          this.initArr[y][x].alive = 0;
        }
      }
    },
  },
  computed: {
    // Блокировка действий кнопок
    blockBtn() {
      let block = true;
      this.initArr.length == 0 || this.activeCellCount == 0
        ? (block = true)
        : (block = false);

      return block;
    },

    // Подсчет активных клеток
    activeCellCount() {
      let total = 0;
      this.initArr.forEach((item) => {
        let sum = item.reduce((accum, item) => accum + item.alive, 0);
        total += sum;
      });
      return total;
    },
  },
};
</script>

<style lang="scss" scoped>
.action {
  width: 60%;
}
</style>
