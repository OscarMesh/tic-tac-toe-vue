<script setup>
import { ref, computed } from "vue";

const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const calculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
};

const winner = computed(() => calculateWinner(board.value.flat()));

const makeMove = (x, y) => {
  if (winner.value) return;
  if (board.value[x][y]) return;
  board.value[x][y] = player.value;
  player.value = player.value === "X" ? "O" : "X";
};

const reset = () => {
  player.value = "X";
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
};
</script>

<template>
  <main class="pt-8 text-center dark:bg-gray-800 min-h-screen dark:text-white">
    <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>
    <h3 class="text-xl mb-4">Player {{ player }}'s turn</h3>
    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="makeMove(x, y)"
          :class="`border border-gray-800 w-20 h-20 hover:bg-gray-600 flex items-center justify-center
      material-icons-outlined text-4xl cursor-pointer ${
        cell === 'X' ? 'text-red-500' : cell === 'O' ? 'text-blue-500' : ''
      }`"
        >
          {{ cell === "X" ? "close" : cell === "O" ? "circle" : "" }}
        </div>
      </div>
    </div>
    <h2 v-if="winner" class="text-6xl font-bold mb-8">
      Player "{{ winner }}" won!
    </h2>
    <button
      @click="reset"
      class="bg-gray-800 text-white px-4 py-2 rounded-md hover:bg-gray-700 duration-300"
    >
      Reset
    </button>
  </main>
</template>

<style scoped></style>
