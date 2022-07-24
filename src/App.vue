<template>
 <section>
  <h2>XO GAME</h2>
  <div class="game-logic">
    <span v-for="i in 9" :key="i" :id='i' @click="clicked(i)"></span>
  </div>
  <div class="winner" v-if="winner != ''">
    <div>The Winner is: <span>{{winner}}</span> </div>
  </div>
  <div class="draw" v-else-if="draw">
    <span>Draw</span>
  </div>
 </section>
 <div class="popup" v-if="openPopup">
  <div class="content">
    <h2>Play XO GAME</h2>
    <p>Choose <span class="x">X</span> or <span class="o">O</span></p>
    <div class="options">
      <div class="x" @click="start('x')">X</div>
      <div class="o" @click="start('o')">O</div>
    </div>
  </div>
 </div>
 <div class="game-finished" v-if="done">
  <button @click="playAgain">Play Again</button>
 </div>
</template>

<script setup>
import {onMounted, reactive, ref} from 'vue'

const playerTurn = ref('')
let added = reactive([])
let winner = ref('')
let openPopup = ref(true)
let done = ref(false)
let count = ref(9)
let draw = ref(false)

function checkWinner() {
  for(let i = 1; i < 10; i++) {
    const span = document.getElementById(i)

    added[i] = span.textContent
  }
  const gameBlock = document.querySelector('.game-logic')
  
  if(added[1] == added[2] && added[1] == added[3] && added[1] != '') {
    winner.value = added[1]
    gameBlock.classList.add('stop')
    done.value = true
  }
  else if(added[4] == added[5] && added[4] == added[6] && added[4] != '') {
    winner.value = added[4]
    gameBlock.classList.add('stop')
    done.value = true
  }
  else if(added[7] == added[8] && added[7] == added[9] && added[7] != '') {
    winner.value = added[1]
    gameBlock.classList.add('stop')
    done.value = true
  }
  else if(added[1] == added[4] && added[1] == added[7] && added[1] != '') {
    winner.value = added[1]
    gameBlock.classList.add('stop')
    done.value = true
  }
  else if(added[2] == added[5] && added[2] == added[8] && added[2] != '') {
    winner.value = added[2]
    gameBlock.classList.add('stop')
    done.value = true
  }
  else if(added[3] == added[6] && added[3] == added[9] && added[3] != '') {
    winner.value = added[3]
    gameBlock.classList.add('stop')
    done.value = true
  }
  else if(added[1] == added[5] && added[1] == added[9] && added[1] != '') {
    winner.value = added[1]
    gameBlock.classList.add('stop')
    done.value = true
  }
  else if(added[3] == added[5] && added[3] == added[7] && added[3] != '') {
    winner.value = added[3]
    gameBlock.classList.add('stop')
    done.value = true
  }
  else if(winner.value == '' && count.value <= 0){
    console.log('draw')
    gameBlock.classList.add('stop')
    done.value = true
    draw.value = true
  }
}

function clicked(id) {
  const span = document.getElementById(id)

  if(span.textContent == '' && playerTurn.value == 'x') {
    span.textContent = 'x'
    playerTurn.value = 'o'
    count.value--
  }
  else if(span.textContent == '' && playerTurn.value == 'o') {
    span.textContent = 'o'
    playerTurn.value = 'x'
    count.value--
  }
  checkWinner()

}

function start(selected) {
  playerTurn.value = selected
  openPopup.value = false
}
function playAgain() {
  window.location.reload()
}

</script>

<style>
 * {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
 }
 body {
  background-color: #f4f4f4;
 }
 section {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
 }
  section h2 {
    margin-bottom: 30px;
  }
 section .game-logic {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  width: 250px;
 }
 section .game-logic span {
  background-color: #333;
  width: 60px;
  height: 60px;
  margin: 5px;
  cursor: pointer;
  font-size: 25px;
  font-weight: bold;
  color: #fff;
  line-height: 2.4;
 }
 section .winner, section .draw {
  height: 5vh;
  margin: 25px 0;
  position: absolute;
  width: 100%;
 }
 section .winner div span {
  font-size: 20px;
  color: #6CC4A1;
  font-weight: bold;
  cursor: default;
 }
 section .draw span {
  background-color: #fff;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: default;
 }
 section .stop {
  pointer-events: none;
 }
 .popup {
  position: relative;
  z-index: 10;
  background: #eee;
  width: 300px;
  height: 500px;
  padding: 20px 0;
 }
 .popup .content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
 }
 .popup p {
  margin: 20px 0;
 }
 .popup .options {
  display: flex;
  justify-content: center;
  align-items: center;
 }
 .popup .options div {
  margin: 20px;
  background-color: #fff;
  padding: 10px 20px;
  border-radius: 5px;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  border: 2px solid transparent;
 }
 .popup .x {
  color: #EB1D36;
 }
 .popup .o {
  color: #4CACBC;
 }

 .popup .x:hover {
  border: 2px solid #EB1D36;
 }
 .popup .o:hover {
  border: 2px solid #4CACBC;
 }
 .game-finished {
  position: relative;
  z-index: 10;
  background-color: #f4f4f488;
  width: 200px;
  height: 200px;
  top: 33.5px;
 }
 .game-finished button {
  background-color: #14C38E;
  color: #fff;
  outline: none;
  border: none;
  margin-top: 80px;
 }
 .game-finished button:hover {
  transform: scale(1.1);
 }
</style>
