
<template>
  <div class="ball" @click="shakeBall">
    <div v-if="!flipped" class="eight-side">
      <div class="eight-circle">
        <span class="eight-text">8</span>
      </div>
      <p class="prompt">Ask me what you want to know</p>
    </div>
    <div v-else class="pyramid-side">
      <svg viewBox="0 0 200 200" class="pyramid-svg">
        <polygon points="100,30 40,170 160,170" fill="#9370DB" />
        <text x="100" y="100" text-anchor="middle" fill="white" font-size="14">{{ answer }}</text>
        <text x="70" y="140" text-anchor="middle" fill="white" font-size="10">{{ otherAnswers[0] }}</text>
        <text x="130" y="140" text-anchor="middle" fill="white" font-size="10">{{ otherAnswers[1] }}</text>
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      flipped: false,
      answer: '',
      otherAnswers: [],
      answers: []
    };
  },
  methods: {
    async fetchAnswers() {
      const res = await fetch('/answers.json');
      const data = await res.json();
      this.answers = data.answers;
    },
    shakeBall() {
      if (this.answers.length > 0) {
        const shuffled = [...this.answers].sort(() => 0.5 - Math.random());
        this.answer = shuffled[0];
        this.otherAnswers = shuffled.slice(1, 3);
        this.flipped = true;
      }
    }
  },
  mounted() {
    this.fetchAnswers();
  }
};
</script>

<style>
.ball {
  width: 300px;
  height: 300px;
  border-radius: 50%;
  background: radial-gradient(circle at center, #000 60%, #222);
  box-shadow: 0 0 30px #000;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  cursor: pointer;
  transition: transform 0.6s ease;
}
.eight-side {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.eight-circle {
  width: 100px;
  height: 100px;
  background: white;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.eight-text {
  font-size: 48px;
  font-weight: bold;
  color: black;
}
.prompt {
  margin-top: 20px;
  font-size: 16px;
}
.pyramid-side {
  width: 100%;
  height: 100%;
}
.pyramid-svg {
  width: 100%;
  height: 100%;
}
</style>
