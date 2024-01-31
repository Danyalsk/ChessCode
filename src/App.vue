<template>
  <div class="w-screen h-screen flex justify-center items-center">
    <div class="flex-col space-y-4">
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
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";
import Select from "./components/Select.vue";

interface Coordinates {
  x: number;
  y: number;
}

const selectedPeice = ref<string>("King");
const validMoves = ref<Coordinates[]>([]);
const selectedCell = ref<Coordinates>({ x: 0, y: 0 });

const handleCellClick = (x: number, y: number) => {
  selectedCell.value = { x, y };
  const soldierMoves = [{ x, y: y + 1 }];
  const kingMoves = [
    { x: x - 1, y: y - 1 },
    { x, y: y - 1 },
    { x: x + 1, y: y - 1 },
    { x: x - 1, y },
    { x: x + 1, y },
    { x: x - 1, y: y + 1 },
    { x, y: y + 1 },
    { x: x + 1, y: y + 1 },
  ];
  const queenMoves = [
    { x: x - 7, y },
    { x: x - 6, y },
    { x: x - 5, y },
    { x: x - 4, y },
    { x: x - 3, y },
    { x: x - 2, y },
    { x: x - 1, y },
    { x: x + 1, y },
    { x: x + 2, y },
    { x: x + 3, y },
    { x: x + 4, y },
    { x: x + 5, y },
    { x: x + 6, y },
    { x: x + 7, y },
    { x, y: y - 7 },
    { x, y: y - 6 },
    { x, y: y - 5 },
    { x, y: y - 4 },
    { x, y: y - 3 },
    { x, y: y - 2 },
    { x, y: y - 1 },
    { x, y: y + 1 },
    { x, y: y + 2 },
    { x, y: y + 3 },
    { x, y: y + 4 },
    { x, y: y + 5 },
    { x, y: y + 6 },
    { x, y: y + 7 },
    { x: x - 7, y: y - 7 },
    { x: x - 6, y: y - 6 },
    { x: x - 5, y: y - 5 },
    { x: x - 4, y: y - 4 },
    { x: x - 3, y: y - 3 },
    { x: x - 2, y: y - 2 },
    { x: x - 1, y: y - 1 },
    { x: x + 1, y: y + 1 },
    { x: x + 2, y: y + 2 },
    { x: x + 3, y: y + 3 },
    { x: x + 4, y: y + 4 },
    { x: x + 5, y: y + 5 },
    { x: x + 6, y: y + 6 },
    { x: x + 7, y: y + 7 },
    { x: x - 7, y: y + 7 },
    { x: x - 6, y: y + 6 },
    { x: x - 5, y: y + 5 },
    { x: x - 4, y: y + 4 },
    { x: x - 3, y: y + 3 },
    { x: x - 2, y: y + 2 },
    { x: x - 1, y: y + 1 },
    { x: x + 1, y: y - 1 },
    { x: x + 2, y: y - 2 },
    { x: x + 3, y: y - 3 },
    { x: x + 4, y: y - 4 },
    { x: x + 5, y: y - 5 },
    { x: x + 6, y: y - 6 },
    { x: x + 7, y: y - 7 },
  ];

  const rookMoves = [
    { x: x - 7, y },
    { x: x - 6, y },
    { x: x - 5, y },
    { x: x - 4, y },
    { x: x - 3, y },
    { x: x - 2, y },
    { x: x - 1, y },
    { x: x + 1, y },
    { x: x + 2, y },
    { x: x + 3, y },
    { x: x + 4, y },
    { x: x + 5, y },
    { x: x + 6, y },
    { x: x + 7, y },
    { x, y: y - 7 },
    { x, y: y - 6 },
    { x, y: y - 5 },
    { x, y: y - 4 },
    { x, y: y - 3 },
    { x, y: y - 2 },
    { x, y: y - 1 },
    { x, y: y + 1 },
    { x, y: y + 2 },
    { x, y: y + 3 },
    { x, y: y + 4 },
    { x, y: y + 5 },
    { x, y: y + 6 },
    { x, y: y + 7 },
  ];

  const bishopMoves = [
    { x: x - 7, y: y - 7 },
    { x: x - 6, y: y - 6 },
    { x: x - 5, y: y - 5 },
    { x: x - 4, y: y - 4 },
    { x: x - 3, y: y - 3 },
    { x: x - 2, y: y - 2 },
    { x: x - 1, y: y - 1 },
    { x: x + 1, y: y + 1 },
    { x: x + 2, y: y + 2 },
    { x: x + 3, y: y + 3 },
    { x: x + 4, y: y + 4 },
    { x: x + 5, y: y + 5 },
    { x: x + 6, y: y + 6 },
    { x: x + 7, y: y + 7 },
    { x: x - 7, y: y + 7 },
    { x: x - 6, y: y + 6 },
    { x: x - 5, y: y + 5 },
    { x: x - 4, y: y + 4 },
    { x: x - 3, y: y + 3 },
    { x: x - 2, y: y + 2 },
    { x: x - 1, y: y + 1 },
    { x: x + 1, y: y - 1 },
    { x: x + 2, y: y - 2 },
    { x: x + 3, y: y - 3 },
    { x: x + 4, y: y - 4 },
    { x: x + 5, y: y - 5 },
    { x: x + 6, y: y - 6 },
    { x: x + 7, y: y - 7 },
  ];

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
