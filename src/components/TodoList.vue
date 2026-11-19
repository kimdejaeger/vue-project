<script setup>
import { ref, watch } from "vue";

// Opgeslagen taken ophalen
const taken = ref(JSON.parse(localStorage.getItem("taken")) || []);
const nieuweTaak = ref("");

// Taken automatisch opslaan
watch(
  taken,
  (nieuweTaken) => {
    localStorage.setItem("taken", JSON.stringify(nieuweTaken));
  },
  { deep: true },
);

// Taak toevoegen
const taakToevoegen = (taak) => {
  if (taak.trim() !== "") {
    taken.value.push(taak.trim());
    nieuweTaak.value = "";
  }
};

// Taak verwijderen
const taakVerwijderen = (index) => {
  taken.value.splice(index, 1);
};
</script>

<template>
  <div class="min-h-screen bg-slate-100 flex items-center justify-center p-8">
    <div class="w-full max-w-2xl bg-white rounded-2xl shadow-xl p-8">
      <div class="mb-8">
        <p class="text-blue-600 font-semibold text-sm mb-2">MIJN PLANNER</p>
        <h1 class="text-3xl font-bold text-slate-800">Mijn takenlijst</h1>
        <p class="text-slate-600 mt-2">Houd je taken bij en blijf georganiseerd.</p>
      </div>
      <!-- Input veld en knop voor het toevoegen van taken -->
      <div class="flex gap-3 mb-6">
        <input
          v-model="nieuweTaak"
          @keyup.enter="taakToevoegen(nieuweTaak)"
          placeholder="Wat moet je nog doen?"
          class="flex-1 min-w-0 bg-white text-slate-900 placeholder:text-slate-500 border border-slate-300 rounded-xl px-4 py-3 outline-none focus:border-blue-600 focus:ring-2 focus:ring-blue-100 transition"
        />
        <button @click="taakToevoegen(nieuweTaak)" class="bg-blue-600 hover:bg-blue-700 text-white font-medium px-5 py-3 rounded-xl transition duration-200">Toevoegen</button>
      </div>

      <div class="flex items-center justify-between mb-4">
        <h2 class="text-lg font-semibold text-slate-800">Mijn taken</h2>
        <span class="bg-blue-100 text-blue-800 text-sm font-medium px-3 py-1 rounded-full"> {{ taken.length }} taken </span>
      </div>
      <!-- Takenlijst -->
      <ul v-if="taken.length > 0" class="space-y-3">
        <li v-for="(taak, index) in taken" :key="index" class="flex items-center justify-between gap-4 bg-slate-50 border border-slate-200 rounded-xl px-4 py-4 hover:border-blue-300 transition">
          <div class="flex items-center gap-3 min-w-0">
            <span class="w-2 h-2 rounded-full bg-blue-500 shrink-0"></span>
            <span class="text-slate-800 break-words">{{ taak }}</span>
          </div>
          <!-- Verwijderknop voor elke taak -->
          <button @click="taakVerwijderen(index)" class="text-red-600 hover:bg-red-50 font-medium text-sm px-3 py-2 rounded-lg transition shrink-0">Verwijderen</button>
        </li>
      </ul>

      <div v-else class="text-center py-10 bg-slate-50 rounded-xl">
        <h2 class="text-lg font-semibold text-slate-800">Geen taken meer!</h2>
        <p class="text-slate-600 text-sm mt-2">Je kunt een nieuwe taak toevoegen.</p>
      </div>
    </div>
  </div>
</template>
