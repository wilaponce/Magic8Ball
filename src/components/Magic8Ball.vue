
<template>
  <div class="magic8-container">
    <div class="magic8" :class="{ shaking: isShaking }" @click="shakeBall">
      <!-- Outer black ball -->
      <svg viewBox="0 0 400 400" class="ball-svg" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <radialGradient id="ballGrad" cx="40%" cy="30%">
            <stop offset="0%" stop-color="#222"/>
            <stop offset="60%" stop-color="#000"/>
            <stop offset="100%" stop-color="#050505"/>
          </radialGradient>
          <filter id="glow" x="-50%" y="-50%" width="200%" height="200%">
            <feGaussianBlur stdDeviation="8" result="b"/>
            <feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge>
          </filter>
        </defs>

        <circle cx="200" cy="200" r="180" fill="url(#ballGrad)" />

        <!-- Inner reflective highlight -->
        <ellipse cx="140" cy="120" rx="50" ry="30" fill="#2a2a2a" opacity="0.5" />

        <!-- Inner dark window (base) -->
        <circle cx="200" cy="200" r="90" fill="#0b0b0b" stroke="#333" stroke-width="4" />

        <!-- Triangle (the 'pyramid' / answer window) -->
        <polygon points="200,135 150,255 250,255" fill="#2b0057" filter="url(#glow)" />

        <!-- White circle behind the '8' that will fade when an answer appears -->
        <circle
          class="white-window"
          cx="200"
          cy="200"
          r="90"
          fill="#fff"
          stroke="#333"
          stroke-width="4"
          :opacity="selectedAnswer ? 0 : 1"
          style="transition: opacity 0.45s ease"
          pointer-events="none"
        />

        <!-- Answer text + big '8' (use opacity to fade between them) -->
        <text
          class="svg-text svg-eight"
          x="200"
          y="205"
          text-anchor="middle"
          fill="#000"
          stroke="#000"
          stroke-width="0"
          font-size="100"
          font-family="Arial, Helvetica, sans-serif"
          font-weight="700"
          :opacity="selectedAnswer ? 0 : 1"
          style="transition: opacity 0.35s ease"
        >
          8
        </text>

        <text
          class="svg-text svg-answer"
          x="200"
          y="210"
          text-anchor="middle"
          fill="#fff"
          font-size="16"
          font-family="Arial"
          :opacity="selectedAnswer ? 1 : 0"
          style="transition: opacity 0.35s ease"
        >
          {{ selectedAnswer }}
        </text>
      </svg>
    </div>
    <div class="instructions">Click the ball to ask a question</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShaking: false,
      answers: [
        'Yes', 'No', 'Maybe', 'Ask again later', 'Definitely',
        'Outlook not so good', 'Without a doubt', 'Cannot predict now'
      ],
      selectedAnswer: ''
    };
  },
  methods: {
    shakeBall() {
      if (this.isShaking) return;
      this.isShaking = true;
      this.selectedAnswer = '';
      // quick shake animation then reveal
      setTimeout(() => {
        this.selectedAnswer = this.answers[Math.floor(Math.random() * this.answers.length)];
        this.isShaking = false;
      }, 900);
    }
  }
};
</script>

<style scoped>
.magic8-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background: #333;
  color: #fff;
}

.magic8 {
  width: 320px;
  height: 320px;
  cursor: pointer;
  transition: transform 0.2s ease;

 

}

.magic8.shaking {
  animation: shake 0.9s ease-in-out;
 transform-origin: center;
  display: inline-block

}

.ball-svg {
  width: 100%;
  height: 100%;
  display: block;
}

.white-window {
  transition: opacity 0.45s ease;
}

.svg-text {
  transition: opacity 0.35s ease;
  pointer-events: none;
}

.instructions {
  margin-top: 18px;
  font-size: 14px;
  opacity: 0.85;
}

@keyframes shake {
  0% {
    transform: translateX(0) translateY(0) rotate(0deg) skewX(0deg);
  }
  15% {
    transform: translateX(-30px) translateY(-10px) rotate(-15deg) skewX(-5deg);
  }
  30% {
    transform: translateX(30px) translateY(10px) rotate(15deg) skewX(5deg);
  }
  45% {
    transform: translateX(-25px) translateY(-8px) rotate(-12deg) skewX(-4deg);
  }
  60% {
    transform: translateX(25px) translateY(8px) rotate(12deg) skewX(4deg);
  }
  75% {
    transform: translateX(-15px) translateY(-5px) rotate(-8deg) skewX(-3deg);
  }
  90% {
    transform: translateX(15px) translateY(5px) rotate(8deg) skewX(3deg);
  }
  100% {
    transform: translateX(0) translateY(0) rotate(0deg) skewX(0deg);
  }
}
</style>
