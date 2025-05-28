<template>
  <div class="flex flex-col justify-center h-screen w-screen overflow-hidden px-80">
    
    <img
      src="./assets/automaksukalkulaator.webp"
      alt="Background"
      class="absolute inset-0 w-full h-full object-cover z-[-1]"
    />

   
      <header class="z-20 absolute top-0 p-4 flex justify-between justify-between items-start w-full max-w-[70%]">
        <img src="./assets/automakskalkulaator_logo.png" alt="Logo" class="h-10" />
        <div class="text-right text-sm mt-2 mr-2">
          <p class="font-semibold text-md text-white">Automaks ei rakendu:</p>
          <ul class="list-disc list-inside text-xs text-white ">
            <li class="py-3">Alarmsõidukid</li>
            <li class="pb-3">Diplomaatilised sõidukid</li>
            <li class="pb-3">Kaitsevaldkonna sõidukid</li>
          </ul>
        </div>
      </header>

       <p class="text-white text-center text-4xl font-bold py-12">Automaksu kalkulaator 2025</p>
      <main class="z-20 bg-white self-center text-black p-8 rounded-xl shadow-lg w-full max-w-[70%] mx-4 text-center">
        <h1 class="text-2xl font-semibold mb-4">Automaksu kalkulaator</h1>
        <p class="text-sm mb-4">
          Automaksu kalkulaator kasutab Transpordiameti andmeid. Arvutatud tulemused on
          informatiivsed ja põhinevad vastuvõetud Mootorsõidukimaksu seadusel, mis hakkab
          kehtima 1. jaanuar 2025. Mootorsõidukimaks koosneb kahest osast: aastamaksust ja
          registreerimistasust. Registreerimistasu makstakse sõiduki registreerimisel
          liiklusregistrisse või sõiduki esimesel omanikuvahetusel. Aastamaksu tasutakse kaks korda aastas.
        </p>
<!--
        <div>
          <div class="flex justify-center mb-4 border-b border-gray-300">
            <button class="px-4 py-2 text-sm border-b-2 border-blue-600 text-blue-600">Registrinumber</button>
            <button class="px-4 py-2 text-sm text-gray-500">Sõiduki baasandmed</button>
          </div>

          
          <div class="flex items-center justify-center mb-4 w-full">
            <span class="bg-blue-600 text-white px-4 py-2 rounded-l">EU</span>
            <input type="text" placeholder="123ABC" class="px-4 py-2 border rounded-r w-full max-w-sm" />
          </div>

          <button class="bg-yellow-300 px-6 py-2 rounded text-black font-semibold">Arvuta</button>
        </div>
      </main>
    </div>
  -->
            <div class="flex justify-center mb-4 border-b border-gray-300">
          <button class="px-4 py-2 text-sm border-b-2 border-blue-600 text-blue-600"
            :class="tab === 'reg' ? 'text-blue-600 border-b-2 border-blue-600' : 'text-gray-600 hover:text-gray-800'"
            @click="tab = 'reg'">
            Registrinumber
          </button>
          <button class="px-4 py-2 text-sm text-gray-500"
            :class="tab === 'data' ? 'text-blue-600 border-b-2 border-blue-600' : 'text-gray-600 hover:text-gray-800'"
            @click="tab = 'data'">
            Sõiduki baasandmed
          </button>
        </div>
        <div v-if="tab === 'reg'" class="mb-6">
          <div class="flex items-center justify-center mb-4 w-full bg-gray-100 rounded-lg">
            <div class="bg-blue-600 text-white px-4 py-2 rounded-l">EU</div>
            <input v-model="regNumber" type="text" placeholder="123ABC"
              class="w-full p-3 outline-none bg-transparent text-gray-800 placeholder-gray-400 uppercase"
              maxlength="6" />
          </div>
        </div>
        <div v-else class="space-y-4 mb-6">
          <div class="text-left">
            <label class="block font-medium mb-2 text-gray-700">Sõiduki tüüp</label>
            <select v-model="vehicleType"
              class="w-full border-1 rounded-lg p-3 bg-white text-gray-800 focus:border-blue-500 focus:outline-none">
              <option value="" ></option>
              <option value="m1">Sõiduauto (M1)</option>
              <option value="n1">Kaubik/veoauto (N1)</option>
              <option value="l3e">Mootorratas/ATV (L3e-, L4e-, L5e-, L6e, L7e)</option>
              <option value="ms2">Ratasmaastikusõiduk (MS2)</option>
              <option value="t1b">Traktor (T1b, T3, T5)</option>
            </select>
          </div>
          <div class="text-left">
            <label class="block font-medium mb-2 text-gray-700">Esmareg. aasta</label>
            <input v-model="firstRegYear" type="number" placeholder="Näiteks: 2019" min="1950"
              :max="new Date().getFullYear()"
              class="w-full border-1 rounded-lg p-3 text-gray-800 focus:border-blue-500 focus:outline-none" />
          </div>
        </div>
        <button
          class="bg-yellow-400 hover:bg-yellow-500 text-white font-semibold px-6 py-3 rounded-lg transition-colors duration-200 shadow-md hover:shadow-lg"
          :disabled="!canCalculate" :class="{ 'opacity-50 cursor-not-allowed': !canCalculate }" @click="calculate">
          Arvuta
        </button>
      </main>
    </div>
      <footer class="z-20 absolute bottom-0 w-full text-xs text-black text-center bg-white bg-opacity-50 p-8">
        Copyright © 2025 Automaksukalkulaator
      </footer>
</template>

<script setup>
import { ref, computed } from 'vue'

const tab = ref('reg')
const regNumber = ref('')
const vehicleType = ref('')
const firstRegYear = ref('')
const showResults = ref(false)

const canCalculate = computed(() => {
  if (tab.value === 'reg') {
    return regNumber.value.length >= 3
  } else {
    return vehicleType.value && firstRegYear.value
  }
})

const calculate = () => {
  if (canCalculate.value) {
    showResults.value = true
    console.log('Calculating...', {
      tab: tab.value,
      regNumber: regNumber.value,
      vehicleType: vehicleType.value,
      firstRegYear: firstRegYear.value
    })
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  overflow: hidden;
}
</style>