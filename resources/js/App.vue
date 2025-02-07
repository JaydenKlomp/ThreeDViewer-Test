<template>
    <div class="container">
        <h1>Auto's te koop</h1>
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
                <tr v-for="(auto, index) in autos" :key="index" @click="openModal(auto)">
                    <td class="clickable">{{ auto.merk }}</td>
                    <td>{{ auto.model }}</td>
                    <td>{{ auto.kleur }}</td>
                    <td>{{ auto.prijs.toLocaleString() }}</td>
                </tr>
            </tbody>
        </table>
        <!-- Modal voor 3D model -->
        <div v-if="modalOpen" class="modal" @click="modalOpen = false">
            <div class="modal-content" @click.stop>
                <span class="close" @click="modalOpen = false">&times;</span>
                <h2>{{ selectedAuto?.merk }} - {{ selectedAuto?.model }}</h2>
                <ThreeDViewer :modelPath="selectedAuto?.modelPath" />
            </div>
        </div>
    </div>
</template>
<script setup>
import { ref } from "vue";
import ThreeDViewer from "./components/ThreeDViewer.vue";

const autos = ref([
    { merk: "Toyota", model: "Corolla", kleur: "Blauw", prijs: 22000, modelPath: "/models/toyotacorolla.glb" },
    { merk: "Volkswagen", model: "Golf", kleur: "Zwart", prijs: 25000, modelPath: "/models/volkswagengolf.glb" },
    { merk: "BMW", model: "3 Serie", kleur: "Wit", prijs: 35000, modelPath: "/models/bmw.glb" },
    { merk: "Mercedes", model: "C-Klasse", kleur: "Grijs", prijs: 40000, modelPath: "/models/mercedes.glb" },
    { merk: "Ford", model: "Focus", kleur: "Rood", prijs: 20000, modelPath: "/models/ford.glb" }
]);

const modalOpen = ref(false);
const selectedAuto = ref(null);

const openModal = (auto) => {
    selectedAuto.value = auto;
    modalOpen.value = true;
};
</script>
<style scoped>
.container {
    max-width: 800px;
    margin: 20px auto;
    text-align: center;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 10px;
}

th,
td {
    padding: 10px;
    border: 1px solid #ddd;
}

th {
    background-color: #f4f4f4;
}

tbody tr:nth-child(even) {
    background-color: #f9f9f9;
}

.clickable {
    cursor: pointer;
    font-weight: bold;
    color: blue;
}

.clickable:hover {
    text-decoration: underline;
}

/* Modal styling */
.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal-content {
    background: white;
    padding: 20px;
    border-radius: 10px;
    text-align: center;
    position: relative;
    width: 50%;
}

.close {
    position: absolute;
    top: 10px;
    right: 15px;
    font-size: 24px;
    cursor: pointer;
}
</style>
