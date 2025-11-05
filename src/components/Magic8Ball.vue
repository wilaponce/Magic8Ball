
<template>
  <div class="container">
    <div
      class="ball"
      :class="{ flipped: isFlipped, shaking: isShaking }"
      @click="shakeBall"
    >
      <div v-if="!isFlipped" class="eight-side">
        <div class="eight-circle">
          <span class="eight-text">8</span>
        </div>
        <div class="prompt">Ask me what you want to know</div>
      </div>
      <div v-else class="pyramid-side">
        <div class="inner-circle">
          <div class="pyramid">
            <div class="answer">{{ selectedAnswer }}</div>
          </div>
          <div class="bubbles">
            <div v-for="n in 10" :key="n" class="bubble" :style="bubbleStyle(n)"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isFlipped: false,
      isShaking: false,
      answers: [
        "Yes", "No", "Maybe", "Ask again later", "Definitely",
        "Outlook not so good", "Without a doubt", "Cannot predict now"
      ],
      selectedAnswer: ""
    };
  },
  methods: {
    shakeBall() {
      if (this.isShaking) return;
      this.isShaking = true;
      setTimeout(() => {
        this.selectedAnswer = this.answers[Math.floor(Math.random() * this.answers.length)];
        this.isFlipped = true;
        this.isShaking = false;
      }, 1000);
    },
    bubbleStyle(n) {
      const size = Math.random() * 8 + 4;
      const left = Math.random() * 100;
      const delay = Math.random() * 2;
      const duration = 3 + Math.random() * 2;
      return {
        width: `${size}px`,
        height: `${size}px`,
        left: `${left}%`,
        animationDelay: `${delay}s`,
        animationDuration: `${duration}s`
      };
    }
  }
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: #111;
}

.ball {
  width: 400px;
  height: 400px;
  border-radius: 50%;
  background: radial-gradient(circle at center, #000 60%, #222);
  box-shadow: 0 0 30px #000;
  position: relative;
  perspective: 1000px;
  transition: transform 1s ease-in-out;
}

.ball.shaking {
  animation: shake 0.5s ease-in-out;
}

.ball.flipped {
  transform: rotateY(180deg);
}

.eight-side, .pyramid-side {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  position: absolute;
  backface-visibility: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.eight-side {
  transform: rotateY(0deg);
}

.pyramid-side {
  transform: rotateY(180deg);
}

.eight-circle {
  width: 100px;
  height: 100px;
  background: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.eight-text {
  font-size: 48px;
  font-weight: bold;
  color: black;
}

.prompt {
  margin-top: 20px;
  color: white;
  font-size: 18px;
}

.inner-circle {
  width: 200px;
  height: 200px;
  background: #4B0082;
  border-radius: 50%;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.pyramid {
  width: 0;
  height: 0;
  border-left: 40px solid transparent;
  border-right: 40px solid transparent;
  border-bottom: 70px solid #9370DB;
  position: relative;
}

.answer {
  position: absolute;
  top: 80px;
  width: 100px;
  text-align: center;
  color: white;
  font-size: 14px;
}

.bubbles {
  position: absolute;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.bubble {
  position: absolute;
  bottom: 0;
  background: white;
  border-radius: 50%;
  opacity: 0.6;
  animation-name: rise;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
}

@keyframes shake {
  0% { transform: rotate(0deg); }
  25% { transform: rotate(10deg); }
  50% { transform: rotate(-10deg); }
  75% { transform: rotate(10deg); }
  100% { transform: rotate(0deg); }
}

@keyframes rise {
  0% { bottom: 0; opacity: 0.6; }
  100% { bottom: 100%; opacity: 0; }
}
</style>
