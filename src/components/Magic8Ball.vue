
<template>
  <div id="app">
    <canvas ref="canvas"></canvas>
    <button @click="shakeBall">Shake</button>
  </div>
</template>

<script>
import * as THREE from 'three';
import { gsap } from 'gsap';
import { Physics2DPlugin } from 'gsap/Physics2DPlugin';

gsap.registerPlugin(Physics2DPlugin);

export default {
  data() {
    return {
      scene: null,
      camera: null,
      renderer: null,
      ball: null,
      pyramids: [],
      selectedPyramid: null,
      shakeSound: new Audio('shake.mp3'),
      revealSound: new Audio('reveal.mp3'),
      answers: [
        "Yes", "No", "Maybe", "Ask again later", "Definitely",
        "Outlook not so good", "Without a doubt", "Cannot predict now"
      ]
    };
  },
  mounted() {
    this.initThree();
    this.animate();
  },
  methods: {
    initThree() {
      const width = window.innerWidth;
      const height = window.innerHeight;

      this.scene = new THREE.Scene();
      this.camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
      this.camera.position.z = 5;

      this.renderer = new THREE.WebGLRenderer({ canvas: this.$refs.canvas, alpha: true });
      this.renderer.setSize(width, height);

      const geometry = new THREE.SphereGeometry(2, 32, 32);
      const material = new THREE.MeshStandardMaterial({ color: 0x000000 });
      this.ball = new THREE.Mesh(geometry, material);
      this.scene.add(this.ball);

      const light = new THREE.PointLight(0xffffff, 1);
      light.position.set(5, 5, 5);
      this.scene.add(light);

      // Create pyramids inside the ball
      for (let i = 0; i < 5; i++) {
        const pyramidGeometry = new THREE.ConeGeometry(0.5, 1, 3);
        const pyramidMaterial = new THREE.MeshStandardMaterial({ color: 0x9370DB });
        const pyramid = new THREE.Mesh(pyramidGeometry, pyramidMaterial);
        pyramid.position.set(Math.random() - 0.5, Math.random() - 0.5, Math.random() - 0.5);
        this.scene.add(pyramid);
        this.pyramids.push(pyramid);
      }
    },
    animate() {
      requestAnimationFrame(this.animate);
      this.pyramids.forEach(pyramid => {
        pyramid.rotation.y += 0.01;
        pyramid.rotation.x += 0.01;
      });
      this.renderer.render(this.scene, this.camera);
    },
    shakeBall() {
      this.shakeSound.play();

      const tl = gsap.timeline({
        onComplete: () => {
          this.revealAnswer();
        }
      });

      tl.to(this.ball.rotation, {
        x: "+=1",
        y: "+=1",
        duration: 0.5,
        ease: "power2.inOut"
      }).to(this.ball.position, {
        y: "-=0.5",
        duration: 0.3,
        yoyo: true,
        repeat: 1,
        ease: "sine.inOut"
      });

      this.pyramids.forEach(pyramid => {
        gsap.to(pyramid.position, {
          physics2D: {
            velocity: Math.random() * 200,
            angle: Math.random() * 360,
            gravity: 200
          },
          duration: 2
        });
      });
    },
    revealAnswer() {
      this.revealSound.play();
      const answer = this.answers[Math.floor(Math.random() * this.answers.length)];
      const pyramidGeometry = new THREE.ConeGeometry(0.5, 1, 3);
      const pyramidMaterial = new THREE.MeshStandardMaterial({ color: 0x9370DB });
      const pyramid = new THREE.Mesh(pyramidGeometry, pyramidMaterial);
      pyramid.position.set(0, 0, 0);
      this.scene.add(pyramid);
      this.selectedPyramid = pyramid;

      const canvas = document.createElement('canvas');
      canvas.width = 256;
      canvas.height = 256;
      const ctx = canvas.getContext('2d');
      ctx.fillStyle = 'white';
      ctx.font = '20px Arial';
      ctx.textAlign = 'center';
      ctx.fillText(answer, 128, 128);

      const texture = new THREE.CanvasTexture(canvas);
      pyramid.material.map = texture;
      pyramid.material.needsUpdate = true;
    }
  }
};
</script>

<style>
#app {
  overflow: hidden;
  margin: 0;
  padding: 0;
}
canvas {
  display: block;
}
button {
  position: absolute;
  top: 20px;
  left: 20px;
  z-index: 10;
}
</style>
