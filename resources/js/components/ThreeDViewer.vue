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

watch(() => props.modelPath, (newModel) => {
    if (model) {
        scene.remove(model); // 🔥 Verwijder het oude model als er een nieuw model komt
    }
    loadModel(newModel); // 🔥 Laad het nieuwe model in de bestaande scene
});

const initThree = () => {
    if (!threeContainer.value) return;

    scene = new THREE.Scene();
    scene.background = new THREE.Color(0xf0f0f0);

    camera = new THREE.PerspectiveCamera(50, 600 / 300, 0.1, 1000);
    camera.position.set(0, 2, 5);

    renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(600, 300);
    renderer.setPixelRatio(window.devicePixelRatio);
    threeContainer.value.appendChild(renderer.domElement);

    const light = new THREE.AmbientLight(0xffffff, 1);
    scene.add(light);

    const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
    directionalLight.position.set(5, 5, 5);
    scene.add(directionalLight);

    controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    controls.dampingFactor = 0.05;
    controls.enableZoom = true;
    controls.minDistance = 4;
    controls.maxDistance = 12;
    controls.enablePan = false;

    controls.addEventListener("start", () => {
        autoRotate = false;
    });

    controls.addEventListener("end", () => {
        autoRotate = true;
    });

    loadModel(props.modelPath);
    animate();
};

const loadModel = (modelPath) => {
    if (!modelPath) return;

    const loader = new GLTFLoader();
    loader.load(modelPath, (gltf) => {
        model = gltf.scene;
        model.position.y -= 0.3;
        scene.add(model);
    });
};

const animate = () => {
    requestAnimationFrame(animate);
    if (autoRotate && model) {
        model.rotation.y += 0.01;
    }
    controls.update();
    renderer.render(scene, camera);
};
</script>
<style scoped>
.three-container {
    width: 600px;
    height: 300px;
    margin: auto;
}
</style>
