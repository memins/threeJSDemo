<template>
  <div>
    <div class="controls">
      <label>Height:</label>
      <input type="number" v-model="height" />
      <button @click="setHeight">Set Height to 1</button>
      <button @click="height++">+</button>
      <button @click="height--">-</button>
    </div>
    <div id="canvas-container"></div>
  </div>
</template>

<script>
import * as THREE from "three";

export default {
  props: {
    geometries: {
      type: Array,
      required: true,
    },
    editableHeight: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      height: this.editableHeight,
    };
  },
  mounted() {
    this.initThree();
  },
  methods: {
    initThree() {
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );
      const renderer = new THREE.WebGLRenderer();
      renderer.setSize(window.innerWidth, window.innerHeight);
      document
        .getElementById("canvas-container")
        .appendChild(renderer.domElement);

      const objects = this.geometries.map((geometry) => {
        const points = geometry.map(
          (point) => new THREE.Vector2(point[0], point[1])
        );
        const latheGeometry = new THREE.LatheGeometry(points);
        const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
        const object = new THREE.Mesh(latheGeometry, material);
        scene.add(object);
        return object;
      });

      camera.position.z = 5;

      const animate = () => {
        requestAnimationFrame(animate);

        renderer.render(scene, camera);
      };

      animate();

      // Watch for changes in the height property and update the objects
      this.$watch("height", (newHeight) => {
        objects.forEach((object) => {
          object.scale.y = newHeight / this.editableHeight;
        });
      });
    },
    setHeight() {
      this.height = 1;
    },
  },
};
</script>

<style scoped>
.controls {
  margin-bottom: 20px;
}
</style>
