<template>
  <div class="wrapper">
    <table class="play-field">
      <tr v-for="(row, i) in gameArr" :key="i">
        <td
          v-for="(cell, j) in row"
          :key="j"
          :class="{ active: cell.alive, blocked: gameLaunched }"
          @click="userAction(i, j)"
        ></td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "PlayField",
  props: {
    sizex: {
      type: Number,
      default: 0,
      required: true,
    },
    sizey: {
      type: Number,
      default: 0,
      required: true,
    },
    gameLaunched: {
      type: Boolean,
      default: false,
      required: true,
    },
  },
  data: () => ({
    gameArr: [],
  }),
  computed: {
    // Отрисовка игрового поля
    renderPlayField() {
      return this.gameArr;
    },
  },
  methods: {
    // Начальная инициализация игрового поля
    initial() {
      for (let y = 0; y < this.sizey; y++) {
        this.$set(this.gameArr, y, []);
        for (let x = 0; x < this.sizex; x++) {
          this.$set(this.gameArr[y], x, { alive: 0 });
        }
      }
    },

    // Действия пользователя (выбор активных клеток)
    userAction(y, x) {
      if (!this.gameLaunched) {
        this.gameArr[y][x].alive
          ? (this.gameArr[y][x].alive = 0)
          : (this.gameArr[y][x].alive = 1);
        this.$emit("update:initArr", this.gameArr);
      }
    },
  },
  created() {
    this.initial();
  },
};
</script>

<style lang="scss" scoped>
.wrapper {
  width: 600px;
  height: 600px;
  padding: 0;
  margin: 0;

  .play-field {
    color: black;
    border: 1px solid black;
    position: relative;
    width: 100%;
    height: 100%;
    border-collapse: collapse;

    td {
      border: 0.5px solid black;
      min-width: 0.1px;
      min-height: 0.1px;
      empty-cells: show;
      cursor: crosshair;
      z-index: 10;
    }

    .active {
      background-color: rgb(206, 200, 200);
    }

    .blocked {
      cursor: not-allowed;
      border: 0.5px solid white;
    }
  }
}
</style>
