<template>
  <div id="app">
    <h1>Тапай чечевицу!</h1>

    <div class="stats-container">
      <div class="timer" :class="{ 'timer-active': gameStarted }">
        {{ timeLeft }}
      </div>
      <div class="timer" :class="{ 'timer-active': gameStarted }">
        {{ clicks }}
      </div>
    </div>

    <div class="button-container">
      <button @click="startGame" v-if="!gameStarted" class="glow-button">
        <img src="./assets/sleepy.png" class="glow-image" />
      </button>
      <button
        @click="increment"
        v-if="gameStarted && !gameOver"
        class="glow-button"
      >
        <img src="./assets/sit.png" class="click-button" />
      </button>

      <div class="restart-container" v-if="gameOver">
        <div class="restart-info">
          <p>Чуню тапнули {{ clicks }} раз!</p>
        </div>
        <button @click="restartGame" class="restart-button">Заново</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      clicks: 0,
      timeLeft: 10,
      gameStarted: false,
      gameOver: false,
      timer: null,
    };
  },
  methods: {
    startGame() {
      this.clicks = 0;
      this.timeLeft = 10;
      this.gameStarted = true;
      this.gameOver = false;

      this.timer = setInterval(() => {
        if (this.timeLeft > 0) {
          this.timeLeft--;
        } else {
          this.endGame();
        }
      }, 1000);
    },
    increment() {
      if (!this.gameOver) {
        this.clicks++;
      }
    },
    endGame() {
      clearInterval(this.timer);
      this.gameOver = true;
    },
    restartGame() {
      clearInterval(this.timer);
      this.clicks = 0;
      this.timeLeft = 10;
      this.gameStarted = false;
      this.gameOver = false;
    },
  },
  beforeDestroy() {
    if (this.timer) {
      clearInterval(this.timer);
    }
  },
};
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: Arial, sans-serif;
  touch-action: manipulation;
}

.stats-container {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
  justify-content: center;
  width: 100%;
  height: auto;
}

.timer {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  font-weight: bold;
  border-radius: 50%;
  background: radial-gradient(
    circle,
    rgb(172, 172, 172) 40%,
    rgb(162, 153, 135) 100%
  );
  box-shadow: 0 0 20px rgba(199, 198, 198, 0.9);
  transition: transform 0.2s ease-in-out;
  border: none;
  color: #222;
}

.timer-active {
  animation: pulse 1s infinite alternate ease-in-out;
}

.button-container {
  position: relative;
  width: 400px;
  height: 400px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.glow-button {
  position: absolute;
  width: 100%;
  height: 100%;
  border: none;
  cursor: pointer;
  background: transparent;
  background: #222;
  border: none;
  padding: 0;
  cursor: pointer;
  transition: 0.3s;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  box-shadow: 0 0 20px rgba(255, 255, 255, 0.6);
  user-select: none;
  touch-action: manipulation;
  user-select: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -webkit-tap-highlight-color: transparent;
}

.glow-button:hover {
  box-shadow: 0 0 40px rgba(255, 255, 255, 0.9);
  transform: scale(1);
}

.glow-image {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: contain;
  transition: 0.3s;
}

.click-button {
  width: 100%;
  height: 100%;
  object-fit: contain;
  border-radius: 50%;
  user-select: none;
}

.restart-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  margin-top: 20px;
}

.restart-info {
  font-size: 6rem;
  font-weight: 600;
  color: rgb(255, 255, 255);
  padding: 10px;
  text-align: center;
  width: 400px;
  min-width: 400px;
  height: 400px;
  min-height: 400px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  white-space: normal;
  word-wrap: break-word;
  text-align: center;
}

.restart-info::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: url("./assets/sleepy.png") center/contain no-repeat;
  filter: brightness(50%);
  z-index: -1;
}

.restart-info p {
  margin: 0;
}

.restart-button {
  font-size: 1.2rem;
  color: white;
  background: #444;
  border-radius: 10px;
  padding: 10px 20px;
  cursor: pointer;
  box-shadow: 0 0 15px rgba(255, 255, 255, 0.6);
  border: none;
  width: auto;
  height: auto;
  min-width: 100px;
}

.restart-button:hover {
  background: #666;
  box-shadow: 0 0 30px rgba(255, 255, 255, 0.9);
}
</style>
