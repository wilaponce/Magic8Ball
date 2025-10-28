
<template>
  <div id="app" @mousedown="startDrag" @mouseup="endDrag" @mousemove="onDrag">
    <div class="ball" :class="animationClass" @animationend="resetAnimation">
      <p :class="textAnimation">{{ answer || "Ask a question and shake, drag, or speak!" }}</p>
    </div>
    <button @click="startListening">🎤 Ask by Voice</button>
    <div class="custom-answers">
      <input v-model="newAnswer" placeholder="Add a custom answer" />
      <button @click="addAnswer">Add</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      answer: "",
      dragging: false,
      lastX: 0,
      lastY: 0,
      answers: [],
      newAnswer: "",
      animationClass: "",
      textAnimation: "",
      lastShakeTime: 0
    };
  },
  methods: {
    async fetchAnswers() {
      const res = await fetch('/answers.json');
      const data = await res.json();
      const localAnswers = JSON.parse(localStorage.getItem('customAnswers') || '[]');
      this.answers = [...data.answers, ...localAnswers];
    },
    getRandomAnswer() {
      if (this.answers.length > 0) {
        const index = Math.floor(Math.random() * this.answers.length);
        this.answer = this.answers[index];
        this.triggerAnimations();
      }
    },
    startDrag(e) {
      this.dragging = true;
      this.lastX = e.clientX;
      this.lastY = e.clientY;
    },
    endDrag() {
      if (this.dragging) {
        this.getRandomAnswer();
      }
      this.dragging = false;
    },
    onDrag(e) {
      if (this.dragging) {
        const dx = Math.abs(e.clientX - this.lastX);
        const dy = Math.abs(e.clientY - this.lastY);
        if (dx > 50 || dy > 50) {
          this.getRandomAnswer();
          this.dragging = false;
        }
      }
    },
    handleShake(event) {
      const acc = event.accelerationIncludingGravity;
      const totalAcc = Math.abs(acc.x) + Math.abs(acc.y) + Math.abs(acc.z);
      const now = Date.now();
      if (totalAcc > 25 && now - this.lastShakeTime > 1000) {
        this.lastShakeTime = now;
        this.getRandomAnswer();
      }
    },
    startListening() {
      const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
      if (!SpeechRecognition) {
        alert("Speech recognition not supported in this browser.");
        return;
      }
      const recognition = new SpeechRecognition();
      recognition.lang = 'en-US';
      recognition.start();
      recognition.onresult = () => {
        this.getRandomAnswer();
      };
    },
    addAnswer() {
      if (this.newAnswer.trim()) {
        const localAnswers = JSON.parse(localStorage.getItem('customAnswers') || '[]');
        localAnswers.push(this.newAnswer.trim());
        localStorage.setItem('customAnswers', JSON.stringify(localAnswers));
        this.answers.push(this.newAnswer.trim());
        this.newAnswer = "";
      }
    },
    triggerAnimations() {
      this.animationClass = "shake pulse flip";
      this.textAnimation = "fade-in bounce";
    },
    resetAnimation() {
      this.animationClass = "";
      this.textAnimation = "";
    }
  },
  mounted() {
    this.fetchAnswers();
    if (window.DeviceMotionEvent) {
      window.addEventListener("devicemotion", this.handleShake);
    }
  },
  beforeUnmount() {
    window.removeEventListener("devicemotion", this.handleShake);
  }
};
</script>

<style>
@import 'https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css';

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background: #111;
  color: white;
  font-family: sans-serif;
  user-select: none;
}
.ball {
  width: 300px;
  height: 300px;
  background: radial-gradient(circle at center, #000 60%, #222);
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  padding: 20px;
  box-shadow: 0 0 30px #000;
}
.shake {
  animation: shake 0.5s ease-in-out;
}
@keyframes shake {
  0% { transform: rotate(0deg); }
  25% { transform: rotate(5deg); }
  50% { transform: rotate(-5deg); }
  75% { transform: rotate(5deg); }
  100% { transform: rotate(0deg); }
}
.pulse {
  animation: pulse 1s infinite;
}
@keyframes pulse {
  0% { box-shadow: 0 0 0px #fff; }
  50% { box-shadow: 0 0 20px #0ff; }
  100% { box-shadow: 0 0 0px #fff; }
}
.flip {
  animation: flip 0.6s ease-in-out;
}
@keyframes flip {
  from { transform: rotateY(0deg); }
  to { transform: rotateY(180deg); }
}
.fade-in {
  animation: fadeIn 0.5s ease-in;
}
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
.bounce {
  animation: bounce 1s;
}
.custom-answers {
  margin-top: 20px;
}
.custom-answers input {
  padding: 5px;
  margin-right: 5px;
}
</style>
