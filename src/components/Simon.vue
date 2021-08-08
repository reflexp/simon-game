<template>
  <main class="game">
    <!-- Starter Screen -->
    <div v-if="stage === 1" class="game__start-screen">
      <Button @btnClick="startGame" btnColor="#ef5777" btnTitle="Start Game"/>
    </div>
    
    <div v-if="stage === 2" class="game__board">
      <div class="game__board-score">
        <h2>Score: {{ score }}</h2>
      </div>

      <div class="game__board-colors">
        <Button v-on="!isLoop ? { click: addColorToUserSequence } : {}" btnTitle="Green" btnClass="green"/>
        <Button v-on="!isLoop ? { click: addColorToUserSequence } : {}" btnTitle="Red" btnClass="red"/>
        <Button v-on="!isLoop ? { click: addColorToUserSequence } : {}" btnTitle="Yellow" btnClass="yellow"/>
        <Button v-on="!isLoop ? { click: addColorToUserSequence } : {}" btnTitle="Blue" btnClass="blue"/>
      </div>
    </div>

    <div v-if="stage === 3" class="game__end-screen">
      <h2>Your Final Score Is: {{ score }}</h2>
      <Button @btnClick="restartGame" btnColor="#ef5777" btnTitle="Restart Game"/>
    </div>
  </main>
</template> 

<script>
import Button from './inputs/Button.vue';

export default {
  /* Component's name */
  name: 'Simon',

  /* Components */
  components: {
    Button
  },

  /* Component's reactive data */
  data() {
    return {
      colors: ['green', 'red', 'yellow', 'blue'],
      gameSequence: [],
      userSequence: [],
      score: 0,
      stage: 1,
      speed: 1,
      time: 800,
      isLoop: false,
    }
  },

  methods: {
    startGame() {
      /* Add animation to outer screen */
      document.querySelector('.game__start-screen').classList.add('active');

      setTimeout(() => { 
        /* Second stage for game */
        this.stage = 2;
      }, 1000);

      setTimeout(() => {
        /* Start game loop */
        this.continueGame();
      }, 2000);
    },

    addColorToGameSequence() {
      /* Add color to game sequence array */
      this.gameSequence.push(this.colors[Math.floor(Math.random() * this.colors.length)]);
      console.log(this.gameSequence);
    },

    addColorToUserSequence(e) {
      /* Add color to user sequence array */
      this.userSequence.push(e.target.classList[0]);
      /* Check user and game to match */
      this.compareUserAndGame();
    },

    highlightColors() {
      /* Set index */
      let index = 0;
      /* Set to true */
      this.isLoop = !this.isLoop;
      /* Loop current colors */
      const interval = setInterval(() => {
        /* Remove active color */
        this.dehighlightColor();
        /* Add active color */
        document.querySelector('.' + this.gameSequence[index]).classList.add('active');
        /* Remove active color */
        setTimeout(() => {
          this.dehighlightColor();
        }, this.time * 0.8 * this.speed)
        /* Iterate index */
        index++;
        /* Play audio */
        const audio = new Audio('https://notificationsounds.com/storage/sounds/file-sounds-1145-when.ogg');
        audio.play();

        if (this.gameSequence.length === index) {
          /* Reset index */
          index = 0;
          /* Clear interval */
          clearInterval(interval);
          /* Set to false */
          this.isLoop = !this.isLoop;

          setTimeout(() => {
            this.dehighlightColor();
          }, this.time * this.speed)
        }
      }, this.time * this.speed);
    },

    dehighlightColor() {
      document.querySelectorAll('.game__board-colors button').forEach(color => color.classList.remove('active'));
    },

    continueGame() {
      this.addColorToGameSequence();

      this.highlightColors();
    },

    compareUserAndGame() {
      if (this.userSequence.length === this.gameSequence.length) {
        /* Continue game */
        if (JSON.stringify(this.gameSequence) === JSON.stringify(this.userSequence)) {
          this.userSequence = [];
          this.score++;
          if (this.score % 2 === 0) this.speed -= .1;
          this.continueGame();
        } 
        /* Game over */
        else 
        {
          /* Game over */
          document.querySelector('.game__board').classList.add('active');
          setTimeout(() => { 
            /* Second stage for game */
            this.stage = 3;
          }, 1000);
        }
      }
    },

    restartGame() {
      window.location.reload();
    }
  }
}
</script>

<style lang="scss">
  $size: 48%;
  $opacity: .65;
  $delay: .2s;

  main.game {
    width: 100vw;
    height: 100vh;
    display: flex;
    padding: 1rem;
    align-items: center;
    justify-content: center;

    .game__start-screen.active {
      animation: 1.1s backOutUp;
    }

    .game__board {
      display: flex;
      max-width: 400px;
      max-height: 400px;
      position: relative;
      animation: 1s backInUp;
      flex-direction: column;
      width: calc(100vw - 2rem);
      height: calc(100vw - 2rem);
      padding-bottom: calc(-60px - .5rem);

      &.active {
        animation: 1.1s backOutUp;
      }

      .game__board-score {
        left: 0;
        width: 100%;
        height: 60px;
        display: flex;
        position: absolute;
        align-items: center;
        justify-content: center;
        top: calc(-60px - 1rem);
        text-transform: uppercase;
      }

      .game__board-colors {
        flex: 1;
        display: flex;
        flex-wrap: wrap;
        border-radius: .5rem;
        align-content: space-between;
        justify-content: space-between;

        div {
          cursor: pointer;
        }

        .green {
          width: $size;
          height: $size;
          opacity: $opacity;
          border-radius: .5rem;
          background-color: #27ae60;
          transition: $delay background-color;

          &.active {
            opacity: 1;
            background-color: #2ecc71;
          }
        }

        .red {
          width: $size;
          height: $size;
          opacity: $opacity;
          border-radius: .5rem;
          background-color: #c0392b;
          transition: $delay background-color;

          &.active {
            opacity: 1;
            background-color: #e74c3c;
          }
        }

        .yellow {
          width: $size;
          height: $size;
          opacity: $opacity;
          border-radius: .5rem;
          background-color: #dab10d;
          transition: $delay background-color;

          &.active {
            opacity: 1;
            background-color: #f1c40f;
          }
        }
      
        .blue {
          width: $size;
          height: $size;
          opacity: $opacity;
          border-radius: .5rem;
          background-color: #2980b9;
          transition: $delay background-color;

          &.active {
            opacity: 1;
            background-color: #3498db;
          }
        }
      }
    }

    .game__end-screen {
      text-align: center;
      animation: 1s backInUp;

      h2 {
        margin-bottom: 1rem;
      }
    }
  }
</style>