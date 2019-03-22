<template>
  <div class="box">
    <button @click="shuffle">重置</button>
    <transition-group name="cell" tag="div" class="container">
      <div
        @click.prevent="clickBlock(index)"
        v-for="(puzzle,index) in puzzles"
        :key="puzzle"
        v-text="puzzle"
        class="cell"
      ></div>
    </transition-group>
  </div>
</template>

<script>
export default {
  data() {
    return {
      puzzles: []
    };
  },
  methods: {
    shuffle() {
      this.puzzles = _.shuffle(this.puzzles);
    },
    rander() {
      this.puzzles = Array.apply(null, { length: 8 }).map(function(_, index) {
        return (index % 8) + 1;
      });
      this.puzzles.push("");
      this.shuffle();
    },
    // 点击方块
    clickBlock(index) {
      let curIndex = this.puzzles[index],
        leftIndex = this.puzzles[index - 1],
        rightIndex = this.puzzles[index + 1],
        topIndex = this.puzzles[index - 3],
        bottomIndex = this.puzzles[index + 3];

      if (leftIndex === "" && index % 3) {
        this.$set(this.puzzles, index - 1, curIndex);
        this.$set(this.puzzles, index, "");
      } else if (rightIndex === "" && 2 !== index % 3) {
        this.$set(this.puzzles, index + 1, curIndex);
        this.$set(this.puzzles, index, "");
      } else if (topIndex === "") {
        this.$set(this.puzzles, index - 3, curIndex);
        this.$set(this.puzzles, index, "");
      } else if (bottomIndex === "") {
        this.$set(this.puzzles, index + 3, curIndex);
        this.$set(this.puzzles, index, "");
      }

      this.pass();
    },

    pass() {
      if (this.puzzles[8] === "") {
        const newPuzzles = this.puzzles.slice(0, 8);

        const isPass = newPuzzles.every((e, i) => e === i + 1);

        if (isPass) {
          alert("666！");
        }
      }
    }
  },
  mounted() {
    this.rander();
  }
};
</script>

<style>
.box {
    width: 400px;
    margin: 60px auto 0;
}

.container {
  display: flex;
  flex-wrap: wrap;
  width: 308px;
  margin-top: 10px;
}
.cell {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100px;
  height: 100px;
  border: 1px solid #aaa;
  margin-right: -1px;
  margin-bottom: -1px;
}
.cell:nth-child(3n) {
  margin-right: 0;
}
.cell:nth-child(27n) {
  margin-bottom: 0;
}
.cell-move {
  transition: transform 1s;
}
</style>
<style scoped>
</style>
