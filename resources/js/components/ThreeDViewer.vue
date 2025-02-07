<template>
    <div ref="threeContainer" class="three-container"></div>
</template>
<script setup>
import { onMounted, ref, watch } from "vue";
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

const props = defineProps({
    modelPath: String
});

const threeContainer = ref(null);
let scene, camera, renderer, model, controls;
let autoRotate = true;

onMounted(() => {
    initThree();
});

watch(() => props.modelPath, () => {
    initThree();
});

const initThree = () => {
    if (threeContainer.value) {
        threeContainer.value.innerHTML = "";
    }

    // Scene Setup
    scene = new THREE.Scene();
    scene.background = new THREE.Color(0xf0f0f0);

    // Camera Setup (🔥 Raise the camera higher)
    camera = new THREE.PerspectiveCamera(50, 600 / 400, 0.1, 1000);
    camera.position.set(0, 2, 6); 

    // Renderer Setup
    renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(600, 400);
    renderer.setPixelRatio(window.devicePixelRatio);
    threeContainer.value.appendChild(renderer.domElement);

    // Lights
    const light = new THREE.AmbientLight(0xffffff, 1);
    scene.add(light);

    const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
    directionalLight.position.set(5, 5, 5);
    scene.add(directionalLight);

    controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    controls.dampingFactor = 0.05;
    controls.enableZoom = true; // Allow zooming
    controls.minDistance = 4; // Prevent too much zoom-in
    controls.maxDistance = 12; // Prevent too much zoom-out
    controls.enablePan = false; // No panning

    // Stop auto-rotation when dragging
    controls.addEventListener("start", () => {
        autoRotate = false;
    });

    // Resume auto-rotation when released
    controls.addEventListener("end", () => {
        autoRotate = true;
    });

    // Load 3D model
    const loader = new GLTFLoader();
    loader.load(props.modelPath, (gltf) => {
        model = gltf.scene;
        model.position.y -= 0.3; // 🔥 Move model slightly lower
        scene.add(model);
        animate();
    });

    // Animation Loop
    const animate = () => {
        requestAnimationFrame(animate);
        if (autoRotate && model) {
            model.rotation.y += 0.01; // Auto-rotation
        }
        controls.update();
        renderer.render(scene, camera);
    };
};
</script>
<style scoped>
.three-container {
    width: 600px;
    height: 400px;
    margin: auto;
}
</style>
