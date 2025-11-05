<template>
  <div class="ball" @click="flipBall">
    <div v-if="!flipped" class="eight-side">
      <div class="eight-circle">
        <span class="eight">8</span>
      </div>
      <p class="prompt">Ask me what you want to know</p>
    </div>
    <div v-else class="pyramid-side">
      <svg width="300" height="300" viewBox="0 0 300 300">
        <defs>
          <linearGradient id="purpleGradient" x1="0" y1="0" x2="0" y2="1">
            <stop offset="0%" stop-color="#800080" />
            <stop offset="100%" stop-color="#4B0082" />
          </linearGradient>
        </defs>
        <circle cx="150" cy="150" r="140" fill="black" />
        <rect x="50" y="180" width="200" height="100" fill="url(#purpleGradient)" rx="20" ry="20" />
        <g v-for="(pyramid, index) in pyramids" :key="index" :transform="'translate(' + pyramid.x + ',' + pyramid.y + ')'">
          <polygon points="0,-30 -25,20 25,20" :fill="pyramid.color">
            <animateTransform attributeName="transform" attributeType="XML"
              type="rotate" from="0" to="360" dur="10s" repeatCount="indefinite"/>
          </polygon>
          <text x="0" y="0" text-anchor="middle" fill="white" font-size="10" font-family="Arial" dy="-10">{{ pyramid.answers[0] }}</text>
          <text x="-20" y="20" fill="white" font-size="10" font-family="Arial">{{ pyramid.answers[1] }}</text>
          <text x="20" y="20" text-anchor="end" fill="white" font-size="10" font-family="Arial">{{ pyramid.answers[2] }}</text>
        </g>
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      flipped: false,
      pyramids: []
    };
  },
  methods: {
    flipBall() {
      this.flipped = true;
      this.loadAnswers();
    },
    async loadAnswers() {
      const res = await fetch('/answers.json');
      const data = await res.json();
      const answers = data.answers;
      this.pyramids = [
        {
          x: 100,
          y: 220,
          color: '#9370DB',
          answers: this.pickAnswers(answers)
        },
        {
          x: 150,
          y: 240,
          color: '#8A2BE2',
          answers: this.pickAnswers(answers)
        },
        {
          x: 200,
          y: 220,
          color: '#BA55D3',
          answers: this.pickAnswers(answers)
        }
      ];
    },
    pickAnswers(answers) {
      const shuffled = [...answers].sort(() => 0.5 - Math.random());
      return shuffled.slice(0, 3);
    }
  }
};
</script>

<style>
.ball {
  width: 300px;
  height: 300px;
  margin: auto;
  margin-top: 50px;
  cursor: pointer;
}
.eight-side {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
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
.eight {
  font-size: 48px;
  font-weight: bold;
  color: black;
}
.prompt {
  margin-top: 20px;
  color: white;
  font-size: 16px;
}
.pyramid-side {
  text-align: center;
}
body {
  background: #111;
  color: white;
  font-family: sans-serif;
}
</style>