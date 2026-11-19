<script setup>
// Vue functies importeren
import { ref, watch, computed } from "vue";

// Opgeslagen taken ophalen uit localStorage
const taken = ref(JSON.parse(localStorage.getItem("taken")) || []);
const nieuweTaak = ref("");

// Berekenen hoeveel taken nog niet zijn afgerond
const openTaken = computed(() => {
  return taken.value.filter((taak) => !taak.afgerond).length;
});

// Taken automatisch opslaan wanneer er iets verandert
watch(
  taken,
  (nieuweTaken) => {
    localStorage.setItem("taken", JSON.stringify(nieuweTaken));
  },
  { deep: true },
);

// Functie om een nieuwe taak toe te voegen
const taakToevoegen = (taak) => {
  if (taak.trim() !== "") {
    taken.value.push({
      tekst: taak.trim(),
      afgerond: false,
    });
    nieuweTaak.value = "";
  }
};

// Functie om een taak uit de lijst te verwijderen
const taakVerwijderen = (index) => {
  taken.value.splice(index, 1);
};
</script>

<template>
  <!-- Hoofdcontainer van de planner -->
  <div class="min-h-screen bg-slate-100 flex items-center justify-center p-8">
    <div class="w-full max-w-2xl bg-white rounded-2xl shadow-xl p-8">
      <!-- Titel en beschrijving -->
      <div class="mb-8">
        <p class="text-blue-600 font-semibold text-sm mb-2">MIJN PLANNER</p>
        <h1 class="text-3xl font-bold text-slate-800">Mijn takenlijst</h1>
        <p class="text-slate-600 mt-2">Houd je taken bij en blijf georganiseerd.</p>
      </div>

      <!-- Inputveld en knop voor het toevoegen van taken -->
      <div class="flex gap-3 mb-6">
        <input
          v-model="nieuweTaak"
          @keyup.enter="taakToevoegen(nieuweTaak)"
          placeholder="Wat moet je nog doen?"
          class="flex-1 min-w-0 bg-white text-slate-900 placeholder:text-slate-500 border border-slate-300 rounded-xl px-4 py-3 outline-none focus:border-blue-600 focus:ring-2 focus:ring-blue-100 transition"
        />
        <button @click="taakToevoegen(nieuweTaak)" class="bg-blue-600 hover:bg-blue-700 text-white font-medium px-5 py-3 rounded-xl transition duration-200">Toevoegen</button>
      </div>

      <!-- Titel en teller van de openstaande taken -->
      <div class="flex items-center justify-between mb-4">
        <h2 class="text-lg font-semibold text-slate-800">Mijn taken</h2>

        <!-- Tonen hoeveel taken nog openstaan -->
        <span class="bg-blue-100 text-blue-800 text-sm font-medium px-3 py-1 rounded-full">
          <span v-if="openTaken > 0"> Je hebt nog {{ openTaken }} {{ openTaken === 1 ? "taak" : "taken" }} open </span>
          <span v-else> Alle taken afgerond! </span>
        </span>
      </div>

      <!-- Takenlijst weergeven als er taken zijn -->
      <ul v-if="taken.length > 0" class="space-y-3">
        <li v-for="(taak, index) in taken" :key="index" class="flex items-center justify-between gap-4 bg-slate-50 border border-slate-200 rounded-xl px-4 py-4 hover:border-blue-300 transition">
          <div class="flex items-center gap-3 min-w-0">
            <!-- Checkbox om een taak af te vinken -->
            <input type="checkbox" v-model="taak.afgerond" class="w-5 h-5 accent-blue-600 cursor-pointer" />

            <!-- Taaktekst met een streep erdoor als deze afgerond is -->
            <span class="text-slate-800 break-words" :class="{ 'line-through text-slate-400': taak.afgerond }">
              {{ taak.tekst }}
            </span>
          </div>

          <!-- Knop om een taak te verwijderen -->
          <button @click="taakVerwijderen(index)" class="text-red-600 hover:bg-red-50 font-medium text-sm px-3 py-2 rounded-lg transition shrink-0">Verwijderen</button>
        </li>
      </ul>

      <!-- Bericht tonen als er geen taken zijn -->
      <div v-else class="text-center py-10 bg-slate-50 rounded-xl">
        <h2 class="text-lg font-semibold text-slate-800">Geen taken meer!</h2>
        <p class="text-slate-600 text-sm mt-2">Je kunt een nieuwe taak toevoegen.</p>
      </div>
    </div>
  </div>
</template>
