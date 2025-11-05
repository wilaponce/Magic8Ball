<template>
  <div class="container" @click="shakeBall">
    <div :class="['ball', { 'shake': isShaking }]">
      <div class="inner-circle">
        <div class="liquid">
          <div v-for="n in 20" :key="n" class="bubble" :style="{ animationDelay: (n * 0.1) + 's', left: (Math.random() * 100) + '%' }"></div>
        </div>
        <div class="pyramids">
          <div class="pyramid background" v-for="n in 3" :key="'bg-' + n" :style="{ top: (30 + n * 10) + '%', left: (30 + n * 10) + '%' }"></div>
          <div class="pyramid foreground">
            <div class="answer">{{ answer }}</div>
          </div>
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
      this.showAnswer = false;
      this.isShaking = true;
      setTimeout(() => {
        this.answer = this.answers[Math.floor(Math.random() * this.answers.length)];
        this.isShaking = false;
        this.showAnswer = true;
      }, 1500);
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
  position: relative;
}

.shake {
  animation: shakeBounce 1.5s ease-in-out;
}

@keyframes shakeBounce {
  0% { transform: rotate(0deg) scale(1); }
  20% { transform: rotate(10deg) scale(1.1); }
  40% { transform: rotate(-10deg) scale(1.1); }
  60% { transform: rotate(10deg) scale(1.1); }
  80% { transform: rotate(-10deg) scale(1.1); }
  100% { transform: rotate(0deg) scale(1); }
}

.inner-circle {
  width: 180px;
  height: 180px;
  background: #4B0082;
  border-radius: 50%;
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.liquid {
  position: absolute;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.bubble {
  position: absolute;
  bottom: 0;
  width: 10px;
  height: 10px;
  background: white;
  border-radius: 50%;
  opacity: 0.6;
  animation: rise 3s infinite ease-in;
}

@keyframes rise {
  0% { transform: translateY(0) scale(1); opacity: 0.6; }
  50% { transform: translateY(-80px) scale(1.2); opacity: 0.4; }
  100% { transform: translateY(-160px) scale(0.8); opacity: 0; }
}

.pyramids {
  position: relative;
  width: 100%;
  height: 100%;
}

.pyramid {
  width: 0;
  height: 0;
  border-left: 30px solid transparent;
  border-right: 30px solid transparent;
  border-bottom: 50px solid #9370DB;
  position: absolute;
}

.pyramid.background {
  opacity: 0.3;
}

.pyramid.foreground {
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

.prompt {
  margin-top: 20px;
  font-size: 18px;
  text-align: center;
}
</style>
