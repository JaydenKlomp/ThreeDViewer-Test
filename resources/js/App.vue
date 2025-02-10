<template>
    <div>
        <!-- nav met de auto merken -->
        <nav class="navbar navbar-expand-lg navbar-dark bg-dark sticky-top">
            <div class="container">
                <a class="navbar-brand d-flex align-items-center" href="/">
                    <span>Cartorial</span>
                </a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse" id="navbarNav">
                    <ul class="navbar-nav ms-3">
                        <li class="nav-item">
                            <a class="nav-link" href="#">Tesla</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#">BMW</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#">Mercedes</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#">Toyota</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#">Audi</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#">Porsche</a>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
        <!-- bovenstuk met banner -->
        <div class="hero-section" :style="{ backgroundImage: `url(./images/homebanner.jpg)` } ">
            <div class=" overlay">
            <h1 class="display-3 fw-bold text-white">test</h1>
            <p class="lead text-white">Ontdek de beste modellen en prijzen.</p>
        </div>
    </div>
    <!-- Autolijst -->
    <div class="container w-75">
        <div class="table-container">
            <h2 class="text-cetner fw-bold">Onze Auto's</h2>
            <div class="table-responsive">
                <table class="table table-hover table-bordered shadow-sm mt-3">
                    <thead class="table-dark">
                        <tr>
                            <th>Merk</th>
                            <th>Model</th>
                            <th>Kleur</th>
                            <th>Prijs (€)</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(auto, index) in autos" :key="index" @mouseenter="hoverModel(auto)" @mouseleave="clearHoverModel" @click="toggleSelectedAuto(auto)">
                            <td class="hoverable">{{ auto.merk }}</td>
                            <td>{{ auto.model }}</td>
                            <td>{{ auto.kleur }}</td>
                            <td>€ {{ auto.prijs.toLocaleString() }}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <!-- 3D Model Viewer -->
        <div v-if="selectedAuto" class="viewer-container">
            <h2>{{ selectedAuto.merk }} - {{ selectedAuto.model }}</h2>
            <ThreeDViewer :modelPath="selectedAuto.modelPath" />
        </div>
    </div>
    <!-- Footer -->
    <footer>
        <p>© 2024 AutoVerkoop. Alle rechten voorbehouden.</p>
    </footer>
    </div>
</template>
<script setup>
import { ref } from "vue";
import ThreeDViewer from "@/components/ThreeDViewer.vue";

const autos = ref([
    { merk: "Toyota", model: "Corolla", kleur: "Blauw", prijs: 22000, modelPath: "/models/toyotacorolla.glb" },
    { merk: "Volkswagen", model: "Golf", kleur: "Zwart", prijs: 25000, modelPath: "/models/volkswagengolf.glb" },
    { merk: "Audi", model: "RS5", kleur: "Wit", prijs: 35000, modelPath: "/models/audir8.glb" },
    { merk: "Mercedes", model: "C-Klasse", kleur: "Grijs", prijs: 40000, modelPath: "/models/benz.glb" },
    { merk: "Ford", model: "Focus", kleur: "Rood", prijs: 20000, modelPath: "/models/ford.glb" }
]);

const selectedAuto = ref(null);
const hoveredAuto = ref(null);

const toggleSelectedAuto = (auto) => {
    if (selectedAuto.value?.merk === auto.merk) {
        selectedAuto.value = null; // Deselecteer auto als er opnieuw op wordt geklikt
    } else {
        selectedAuto.value = auto; // Selecteer nieuwe auto
    }
};

const hoverModel = (auto) => {
    if (!selectedAuto.value) {
        hoveredAuto.value = auto;
    }
};

const clearHoverModel = () => {
    if (!selectedAuto.value) {
        hoveredAuto.value = null;
    }
};
</script>