<script setup>
import { ref } from 'vue'

const box = ref(Array(9).fill(null))
const xIsNext = ref(true)
const winner = ref()

const handleClick = (index) => {
  if (winner.value || box.value[index]) return

  box.value[index] = xIsNext.value ? 'X' : 'O'
  xIsNext.value = !xIsNext.value
  getWinner(box.value)
}

const resetGame = () => {
  winner.value = null
  xIsNext.value = true
  box.value = Array(9).fill(null)
}

// get winner
const getWinner = (box) => {
  const arr = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ]

  for (let i = 0; i < arr.length; i++) {
    const [a, b, c] = arr[i]
    if (box[a] && box[a] === box[b] && box[a] === box[c]) return (winner.value = box[a])
  }
  return (winner.value = null)
}
</script>

<template>
  <div>
    <h1>Tic Tac Toe</h1>
    <h2 v-if="winner" style="color: #12e624; font-weight: 700">{{ winner }} is winner!</h2>
    <h2 v-else>{{ xIsNext ? 'X' : 'O' }}'s Turn</h2>
    <div class="box" :class="{ won: winner }">
      <span v-for="(square, index) in box" :key="index" @click="handleClick(index)">
        {{ square }}
      </span>
    </div>
    <button @click="resetGame" class="reset">Restart</button>
  </div>
</template>

<style scoped>
.box {
  display: grid;
  grid-template-columns: repeat(3, 100px);
}
.box span {
  width: 100px;
  height: 100px;
  font-size: 40px;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #000;
  cursor: pointer;
}
.won span {
  cursor: default;
  background: #e4e4e4;
}
.reset {
  font-size: 18px;
  margin: 10px;
  padding: 10px 20px;
  cursor: pointer;
}
</style>
