<template>
      <span class="display-1">Floor</span>
      <Transition name="slide-fade" mode="out-in">
        <h1 class="display-1" :key=floor>{{ floor }}</h1>
      </Transition>
      <!-- Menu -->
      <div class="menu" v-if="showMenu">
        <ul class="nav flex-column">
          <li class="nav-item">
            <a class="nav-link" @click="startGame()" href="#">>Start</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#" data-toggle="modal" data-target="#rules">>Rules</a>
          </li>
        </ul>
        <Rules/>
      </div>
      <!-- Game -->
      <div v-if="!endGame && !showMenu" >
        <div class="progress">
          <div class=progress-bar :class="computedHealthClass" :style="{ width: health + '%' }" role="progressbar"></div>
        </div>
        <div class="progress">
          <div class="progress-bar progress-bar-striped progress-bar-animated" :class="computedTimerClass" :style="{ width: timer + '%'}" role="progressbar"></div>
        </div>

        <Floor :key="componentKey" @goToNextFloor="updateFloor()" @deductHealth="deductHealth()"/>
      </div>
      <!-- Results -->  
      <div v-else-if="endGame">
        <p v-if="floor > 0">
          Game Over!
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-emoji-frown" viewBox="0 0 16 16">
          <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14m0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16"/>
          <path d="M4.285 12.433a.5.5 0 0 0 .683-.183A3.5 3.5 0 0 1 8 10.5c1.295 0 2.426.703 3.032 1.75a.5.5 0 0 0 .866-.5A4.5 4.5 0 0 0 8 9.5a4.5 4.5 0 0 0-3.898 2.25.5.5 0 0 0 .183.683M7 6.5C7 7.328 6.552 8 6 8s-1-.672-1-1.5S5.448 5 6 5s1 .672 1 1.5m4 0c0 .828-.448 1.5-1 1.5s-1-.672-1-1.5S9.448 5 10 5s1 .672 1 1.5"/>
          </svg>
        </p>
        <p v-else>
          Well done! You managed to escape in {{ _countdown-countdownTracker }}s!
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-emoji-smile" viewBox="0 0 16 16">
          <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14m0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16"/>
          <path d="M4.285 9.567a.5.5 0 0 1 .683.183A3.5 3.5 0 0 0 8 11.5a3.5 3.5 0 0 0 3.032-1.75.5.5 0 1 1 .866.5A4.5 4.5 0 0 1 8 12.5a4.5 4.5 0 0 1-3.898-2.25.5.5 0 0 1 .183-.683M7 6.5C7 7.328 6.552 8 6 8s-1-.672-1-1.5S5.448 5 6 5s1 .672 1 1.5m4 0c0 .828-.448 1.5-1 1.5s-1-.672-1-1.5S9.448 5 10 5s1 .672 1 1.5"/>
        </svg>
        </p>

        <p v-if="timer <= 0">You are out of time! 
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-hourglass" viewBox="0 0 16 16">
          <path d="M2 1.5a.5.5 0 0 1 .5-.5h11a.5.5 0 0 1 0 1h-1v1a4.5 4.5 0 0 1-2.557 4.06c-.29.139-.443.377-.443.59v.7c0 .213.154.451.443.59A4.5 4.5 0 0 1 12.5 13v1h1a.5.5 0 0 1 0 1h-11a.5.5 0 1 1 0-1h1v-1a4.5 4.5 0 0 1 2.557-4.06c.29-.139.443-.377.443-.59v-.7c0-.213-.154-.451-.443-.59A4.5 4.5 0 0 1 3.5 3V2h-1a.5.5 0 0 1-.5-.5m2.5.5v1a3.5 3.5 0 0 0 1.989 3.158c.533.256 1.011.791 1.011 1.491v.702c0 .7-.478 1.235-1.011 1.491A3.5 3.5 0 0 0 4.5 13v1h7v-1a3.5 3.5 0 0 0-1.989-3.158C8.978 9.586 8.5 9.052 8.5 8.351v-.702c0-.7.478-1.235 1.011-1.491A3.5 3.5 0 0 0 11.5 3V2z"/>
          </svg>
        </p>

        <p v-if="health <= 0">You are out of energy! 
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-lightning-charge" viewBox="0 0 16 16">
          <path d="M11.251.068a.5.5 0 0 1 .227.58L9.677 6.5H13a.5.5 0 0 1 .364.843l-8 8.5a.5.5 0 0 1-.842-.49L6.323 9.5H3a.5.5 0 0 1-.364-.843l8-8.5a.5.5 0 0 1 .615-.09zM4.157 8.5H7a.5.5 0 0 1 .478.647L6.11 13.59l5.732-6.09H9a.5.5 0 0 1-.478-.647L9.89 2.41z"/>
          </svg>
        </p>
  
        <button type="button" class="btn btn-secondary" @click="resetGame()">Try Again?</button>
      </div>
</template>

<script>
  import Floor from './components/Floor.vue'
  import Rules from './components/Rules.vue'
  import { ref, computed, watchEffect } from 'vue' 

  export default {
    name : 'App',
    components: {Floor, Rules},
    
    setup(){
      //settings
      const _numFloors = 10;
      const _countdown = 150;
      const _full = 100;
      let _timerId = null;

      const floor = ref(_numFloors);
      const componentKey = ref(_numFloors);
      const countdownTracker = ref(_countdown);
      const showMenu = ref(true);
      const endGame = ref(false);
      const health = ref(_full);
      const timer = ref(_full);

      //computed
      const computedHealthClass = computed(() => {return health.value <= 20 ? 'bg-danger' : health.value <= 50 ? 'bg-warning' : 'bg-success'});
      const computedTimerClass = computed(() => {return timer.value <= 10 ? 'bg-danger' : timer.value <= 50 ? 'bg-warning' : 'bg-primary'});

      watchEffect(() => {
        if (timer.value <= 0 || health.value <= 0){
          stopTime();
          endGame.value = true;
        }
      })

      //methods
      function startGame(){
        showMenu.value = false;
        activateTime();
      }
      
      function updateFloor(){
        floor.value--;

        if(floor.value === 0){
          endGame.value = true;
          stopTime();
        }else{
          componentKey.value = floor.value;
        }
      }

      function deductHealth(){
        health.value = health.value - 20;
      }

      function activateTime(){
        if (timer.value > 0) {
          _timerId = setInterval(() => {
            countdownTracker.value--;
            timer.value = (countdownTracker.value / _countdown) * 100;
          }, 1000);
        }
      }

      function stopTime(){
        clearInterval(_timerId);
      }
      
      function resetGame(){
        floor.value = _numFloors;
        componentKey.value = _numFloors;
        countdownTracker.value = _countdown;
        health.value = _full;
        timer.value = _full;
        endGame.value = false;

        activateTime();
      }

      return { _countdown, showMenu, floor, endGame, health, componentKey, computedHealthClass, computedTimerClass, timer, countdownTracker, 
        startGame, updateFloor, deductHealth, activateTime, stopTime, resetGame}
    }

  }
</script>

<style>

  #app {
    text-align: center;
    max-width: 1280px;
    margin: 0 auto;
    padding: 2rem;
    font-weight: normal;
  }

  .menu {
    text-align: left;
    justify-self: center;
    max-width: 50%;
    font-size: 20px;
  }

  .slide-fade-enter-active {
  transition: all 0.3s ease;
}

.slide-fade-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(20px);
  opacity: 0;
}


  /* Chrome, Safari, Edge, Opera */
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
      -webkit-appearance: none;
      margin: 0;
  }
  
  .puzzle {
      padding-top : 10px ;
      font-size : 30px;
      margin-right : auto;
      margin-left : auto;
  }

  .puzzle input{
      width :100px;
  }

  #keyCollection {
    padding-top : 10px;
  }

  .progress {
    margin: 10px;
  }

</style>
