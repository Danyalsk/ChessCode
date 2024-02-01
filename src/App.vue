<template>
  <div
    class="w-screen h-screen flex justify-center items-center bg-gradient-to-r from-slate-900 to-slate-700"
  >
    <div class="flex-col space-y-4 bg-white p-4 border border-black rounded-md">
      <h1
        class="text-2xl font-bold justify-center items-center flex bg-gradient-to-r from-slate-900 to-slate-700 bg-clip-text text-transparent"
      >
        Chess Moves Finder
      </h1>
      <Select v-model="selectedPeice" />
      <div class="flex-col">
        <div v-for="y in 8" :key="y" class="flex">
          <div
            v-for="x in 8"
            :key="x"
            class="p-4 md:p-8 border border-black flex items-center justify-center"
            :style="
              selectedCell.x === x && selectedCell.y === y
                ? 'background-color: lightblue'
                : validMoves.some((move) => move.x === x && move.y === y)
                ? 'background-color: lightgreen'
                : ''
            "
            @click="handleCellClick(x, y)"
          ></div>
        </div>
        <div class="p-1">
          Number of possible moves :{{ filteredValidMoves }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from "vue";
import Select from "./components/Select.vue";

interface Coordinates {
  x: number;
  y: number;
}

const selectedPeice = ref<string>("Soldier");
const validMoves = ref<Coordinates[]>([]);
const selectedCell = ref<Coordinates>({ x: 0, y: 0 });

const filteredValidMoves = computed(
  () =>
    validMoves.value.filter(
      (move) => move.x > 0 && move.x <= 8 && move.y > 0 && move.y <= 8
    ).length
);

const handleCellClick = (x: number, y: number) => {
  selectedCell.value = { x, y };
  const soldierMoves = [{ x, y: y - 1 }];
  const kingMoves = [];

  for (let i = -1; i <= 1; i++) {
    for (let j = -1; j <= 1; j++) {
      if (i !== 0 || j !== 0) {
        kingMoves.push({ x: x + i, y: y + j });
      }
    }
  }

  const queenMoves = [];

  for (let i = -7; i <= 7; i++) {
    queenMoves.push({ x: x + i, y });
    queenMoves.push({ x, y: y + i });
    queenMoves.push({ x: x - i, y });
    queenMoves.push({ x, y: y - i });

    queenMoves.push({ x: x + i, y: y + i });
    queenMoves.push({ x: x - i, y: y + i });
    queenMoves.push({ x: x + i, y: y - i });
    queenMoves.push({ x: x - i, y: y - i });
  }

  const rookMoves = [];

  for (let i = -7; i <= 7; i++) {
    if (i !== 0) {
      rookMoves.push({ x: x + i, y }, { x, y: y + i });
    }
  }

  const bishopMoves = [];

  for (let i = -7; i <= 7; i++) {
    if (i !== 0) {
      bishopMoves.push(
        { x: x + i, y: y + i },
        { x: x - i, y: y - i },
        { x: x - i, y: y + i },
        { x: x + i, y: y - i }
      );
    }
  }

  const horseMoves = [
    { x: x - 2, y: y - 1 },
    { x: x - 1, y: y - 2 },
    { x: x + 1, y: y - 2 },
    { x: x + 2, y: y - 1 },
    { x: x - 2, y: y + 1 },
    { x: x - 1, y: y + 2 },
    { x: x + 1, y: y + 2 },
    { x: x + 2, y: y + 1 },
  ];
  switch (selectedPeice.value) {
    case "Soldier":
      validMoves.value = soldierMoves;
      break;
    case "King":
      validMoves.value = kingMoves;
      break;
    case "Queen":
      validMoves.value = queenMoves;
      break;
    case "Rook":
      validMoves.value = rookMoves;
      break;
    case "Bishop":
      validMoves.value = bishopMoves;
      break;
    case "Horse":
      validMoves.value = horseMoves;
      break;
    default:
      validMoves.value = [];
      break;
  }
};

watch(selectedPeice, () => {
  validMoves.value = [];
  selectedCell.value = { x: 0, y: 0 };
});
</script>
