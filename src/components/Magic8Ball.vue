
<template>
  <div class="container" @click="shakeBall">
    <div :class="['ball', { 'shake': isShaking }]">
      <div class="inner-circle" v-if="showAnswer">
        <div class="pyramid">
          <div class="answer">{{ answer }}</div>
        </div>
        <div class="bubbles">
          <div v-for="n in 10" :key="n" class="bubble" :style="{ animationDelay: (n * 0.2) + 's' }"></div>
        </div>
      </div>
    </div>
    <div v-if="!showAnswer" class="prompt">Ask me what you want to know</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShaking: false,
      showAnswer: false,
      answer: '',
      answers: [
        "Yes", "No", "Maybe", "Ask again later", "Definitely",
        "Outlook not so good", "Without a doubt", "Cannot predict now"
      ]
    };
  },
  methods: {
    shakeBall() {
      if (this.isShaking) return;
      this.isShaking = true;
      setTimeout(() => {
        this.isShaking = false;
        this.showAnswer = true;
        this.answer = this.answers[Math.floor(Math.random() * this.answers.length)];
      }, 1000);
    }
  }
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background: radial-gradient(circle, #222, #000);
  color: white;
  font-family: sans-serif;
  user-select: none;
}

.ball {
  width: 300px;
  height: 300px;
  background: radial-gradient(circle at center, #000 60%, #222);
  border-radius: 50%;
  box-shadow: 0 0 30px #000;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.5s ease;
}

.shake {
  animation: shake 0.5s ease-in-out;
}

@keyframes shake {
  0% { transform: rotate(0deg) scale(1); }
  25% { transform: rotate(5deg) scale(1.05); }
  50% { transform: rotate(-5deg) scale(1.05); }
  75% { transform: rotate(5deg) scale(1.05); }
  100% { transform: rotate(0deg) scale(1); }
}

.inner-circle {
  width: 180px;
  height: 180px;
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
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.answer {
  position: absolute;
  top: 60%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  font-size: 14px;
  text-align: center;
  width: 100px;
}

.bubbles {
  position: absolute;
  bottom: 10px;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.bubble {
  position: absolute;
  bottom: 0;
  left: 50%;
  width: 10px;
  height: 10px;
  background: white;
  border-radius: 50%;
  opacity: 0.6;
  animation: rise 3s infinite ease-in;
}

@keyframes rise {
  0% { transform: translateY(0) scale(1); opacity: 0.6; }
  50% { transform: translateY(-100px) scale(1.2); opacity: 0.4; }
  100% { transform: translateY(-200px) scale(0.8); opacity: 0; }
}

.prompt {
  margin-top: 20px;
  font-size: 18px;
  text-align: center;
}
</style>
