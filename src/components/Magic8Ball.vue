
<template>
  <div class="container" @click="shakeBall">
    <div ref="ball" class="ball">
      <div v-if="!showAnswer" class="eight">8</div>
      <div v-if="showAnswer" class="inner-circle">
        <div v-for="(pyramid, index) in pyramids" :key="index" class="pyramid" :ref="'pyramid' + index">
          <div class="face" v-for="(face, fIndex) in 3" :key="fIndex">
            <div class="answer">{{ index === selectedIndex ? answer : '' }}</div>
          </div>
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
import { gsap } from "gsap";

export default {
  data() {
    return {
      showAnswer: false,
      answer: '',
      answers: [
        "Yes", "No", "Maybe", "Ask again later", "Definitely",
        "Outlook not so good", "Without a doubt", "Cannot predict now"
      ],
      pyramids: new Array(5).fill(null),
      selectedIndex: 0
    };
  },
  methods: {
    shakeBall() {
      if (this.showAnswer) return;

      this.answer = this.answers[Math.floor(Math.random() * this.answers.length)];
      this.selectedIndex = Math.floor(Math.random() * this.pyramids.length);
      this.showAnswer = true;

      this.$nextTick(() => {
        const ball = this.$refs.ball;
        const tl = gsap.timeline({ defaults: { duration: 0.5, ease: "power2.inOut" } });

        tl.to(ball, { rotation: 15, scale: 1.05 })
          .to(ball, { rotation: -15, scale: 1.05 })
          .to(ball, { rotation: 0, scale: 1 });

        this.pyramids.forEach((_, index) => {
          const pyramid = this.$refs['pyramid' + index];
          const pyramidTl = gsap.timeline({ repeat: -1, yoyo: true, ease: "sine.inOut" });
          pyramidTl.to(pyramid, { rotationY: 360, duration: 3 });
          pyramidTl.to(pyramid, { x: Math.random() * 100 - 50, y: Math.random() * 100 - 50, duration: 2 });
        });

        const selectedPyramid = this.$refs['pyramid' + this.selectedIndex];
        gsap.to(selectedPyramid, {
          x: 0,
          y: -50,
          scale: 1.2,
          duration: 2,
          ease: "elastic.out(1, 0.3)"
        });
      });
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
  position: relative;
  transform-style: preserve-3d;
}

.eight {
  font-size: 100px;
  color: white;
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
  overflow: hidden;
}

.pyramid {
  position: absolute;
  width: 0;
  height: 0;
  transform-style: preserve-3d;
}

.face {
  width: 0;
  height: 0;
  border-left: 40px solid transparent;
  border-right: 40px solid transparent;
  border-bottom: 70px solid #9370DB;
  position: absolute;
  transform-origin: center;
}

.answer {
  position: absolute;
  top: 40%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  font-size: 12px;
  text-align: center;
  width: 80px;
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
