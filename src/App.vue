<script setup lang="ts">

import { ref } from 'vue';

const currentValue = ref('X');
const txt = ref('Move: ');
const isEnable = ref(false);

const matrixMove = ref ([
  [' ',' ',' '],
  [' ',' ',' '],
  [' ',' ',' ']
]);

function Move()
{
  currentValue.value = currentValue.value === 'O' ? 'X' : 'O';
  console.log('now: ' + currentValue.value);
  return currentValue.value;
}

function MoveClick(rowIndex, colIndex)
{
  if (matrixMove.value[rowIndex][colIndex] == ' ')
  {
    matrixMove.value[rowIndex][colIndex] = currentValue.value;
    const winner = checkWinner();
    if (winner) 
    {
      txt.value = 'WIN: ';
      isEnable.value = true;
    } 
    else 
    {
      Move();
    }
    console.log('Znachenie', matrixMove.value[rowIndex][colIndex]);
}
}

function checkWinner() 
{
  const lines = [
    // Горизонтальные комбинации
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    // Вертикальные комбинации
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    // Диагональные комбинации
    [0, 4, 8],
    [2, 4, 6]
  ];

  // Проверяем все возможные комбинации
  for (let i = 0; i < lines.length; i++) 
  {
    const [a, b, c] = lines[i];
    const valueA = matrixMove.value[Math.floor(a / 3)][a % 3];
    const valueB = matrixMove.value[Math.floor(b / 3)][b % 3];
    const valueC = matrixMove.value[Math.floor(c / 3)][c % 3];

    // Если все три значения в комбинации равны и не пусты, то есть победитель
    if (valueA !== ' ' && valueA === valueB && valueB === valueC) 
    {
      return valueA;
    }
  }

  return false;
}

function restartGame() 
{
  currentValue.value = 'X';
  txt.value = 'Move: ';
  matrixMove.value = [
    [' ', ' ', ' '],
    [' ', ' ', ' '],
    [' ', ' ', ' ']
  ];
}

</script>

<template>
  <h1>Krestiki-Noliki</h1>
  <h3>{{txt + currentValue }}</h3>
  <div >
    <div class="game-board" v-for="(row, rowIndex) in matrixMove" :key="rowIndex">
      <button class="btn success" 
              v-for="(value, colIndex) in row" :key="colIndex" 
              @click="MoveClick(rowIndex, colIndex)"
              :disabled="isEnable">{{ value }}</button>
    </div>
  </div>

  <p></p>
  <button @click="restartGame"> Restart </button>

</template>

<style scoped>

.game-board
{
  display: flex;
  align-items: center;
  justify-content: center;
}
.btn
{
  width: 150px;
  height: 150px;
  border: 2px solid black;
  padding: 14px 28px;
  cursor: pointer;
  border-radius: 10px;
  font-size: xxx-large;
}

.success {
  border-color: black;
  color: gray;
}

</style>

