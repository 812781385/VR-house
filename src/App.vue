<template>
  <div class="container" ref="container"></div>
</template>

<script setup lang="ts">
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { onMounted, ref } from "vue";

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.z = .1;
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);

const container: any = ref(null);

const render = () => {
  renderer.render(scene, camera);
  requestAnimationFrame(render);
};

const geometry = new THREE.BoxGeometry(10, 10, 10);
const arr = ['rightImg', 'leftImg', 'topImg', 'bottomImg', 'frontImg', 'backImg'];
const boxMaterials: any = [];

arr.forEach((item: any) => {
  let texture = new THREE.TextureLoader().load(`/${item}.jpg`);
  
  if (item === 'topImg') {
    texture.rotation = - Math.PI / 2;
  }
  if (item === 'bottomImg') {
    texture.rotation = Math.PI / 2;
  }

  texture.center = new THREE.Vector2(0.5, 0.5);
  boxMaterials.push(new THREE.MeshBasicMaterial({ map: texture }));
});
const cube = new THREE.Mesh(geometry, boxMaterials);
cube.geometry.scale(1, 1, -1);
scene.add(cube);

onMounted(() => {
  const controls = new OrbitControls(camera, container.value);
  controls.enableDamping = true;
  container.value.appendChild(renderer.domElement);
  render();
  
});

</script>

<style scoped lang="scss">
</style>
