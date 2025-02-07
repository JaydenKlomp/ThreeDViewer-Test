<template>
    <div class="container">
        <!-- Hero Banner -->
        <header class="hero">
            <div class="hero-content">
                <h1>Vind jouw droomauto</h1>
                <p>Ontdek ons uitgebreide aanbod en bekijk modellen in 3D!</p>
                <button class="cta-button">Bekijk aanbod</button>
            </div>
        </header>
        <!-- Autolijst -->
        <div class="content">
            <div class="table-container">
                <h2>Onze Auto's</h2>
                <table>
                    <thead>
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
    { merk: "BMW", model: "3 Serie", kleur: "Wit", prijs: 35000, modelPath: "/models/bmw.glb" },
    { merk: "Mercedes", model: "C-Klasse", kleur: "Grijs", prijs: 40000, modelPath: "/models/mercedes.glb" },
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