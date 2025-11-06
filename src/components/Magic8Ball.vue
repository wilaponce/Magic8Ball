<template>
  <div class="container">
    <canvas ref="canvas" class="three-canvas"></canvas>
  </div>
</template>

<script>
import { onMounted, ref } from 'vue';
import * as THREE from 'three';
import { gsap } from 'gsap';

export default {
  setup() {
    const canvas = ref(null);

    onMounted(() => {
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      const renderer = new THREE.WebGLRenderer({ canvas: canvas.value, alpha: true });
      renderer.setSize(window.innerWidth, window.innerHeight);
      renderer.shadowMap.enabled = true;

      const ballGeometry = new THREE.SphereGeometry(2, 32, 32);
      const ballMaterial = new THREE.MeshStandardMaterial({ color: 0x000000 });
      const ball = new THREE.Mesh(ballGeometry, ballMaterial);
      ball.castShadow = true;
      scene.add(ball);

      const pyramidGeometry = new THREE.ConeGeometry(1, 2, 4);
      const pyramidMaterial = new THREE.MeshStandardMaterial({ color: 0x9370DB });
      const pyramid = new THREE.Mesh(pyramidGeometry, pyramidMaterial);
      pyramid.position.set(0, 0, 2);
      pyramid.castShadow = true;
      scene.add(pyramid);

      const light = new THREE.PointLight(0xffffff, 1);
      light.position.set(5, 5, 5);
      light.castShadow = true;
      scene.add(light);

      const ambientLight = new THREE.AmbientLight(0x404040);
      scene.add(ambientLight);

      camera.position.z = 5;

      function animate() {
        requestAnimationFrame(animate);
        renderer.render(scene, camera);
      }

      animate();

      gsap.to(ball.rotation, {
        x: Math.PI * 2,
        y: Math.PI * 2,
        duration: 2,
        ease: "power2.inOut"
      });

      gsap.to(pyramid.rotation, {
        y: Math.PI * 2,
        duration: 3,
        ease: "elastic.out(1, 0.3)"
      });
    });

    return {
      canvas
    };
  }
};
</script>

<style scoped>
.container {
  width: 100%;
  height: 100vh;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  background: radial-gradient(circle, #222, #000);
}

.three-canvas {
  width: 100%;
  height: 100%;
  display: block;
}
</style>
