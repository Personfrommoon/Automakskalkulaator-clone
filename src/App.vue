<template>
  <div class="relative flex flex-col min-h-screen w-screen overflow-y-auto text-wrap px-4 md:px-80 pb-40">
    <img src="./assets/automaksukalkulaator.webp" alt="Background"
      class="absolute inset-0 w-full h-full object-cover z-[-1]" />

    <header class="z-20 absolute top-0 p-4 flex justify-between items-start w-full max-w-[70%]">
      <img src="./assets/automakskalkulaator_logo.png" alt="Logo" class="h-10" />
      <div class="text-right text-sm mt-2 mr-2">
        <p class="font-semibold text-md text-white">Automaks ei rakendu:</p>
        <ul class="list-disc list-inside text-xs text-white">
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
        Automaksu kalkulaator kasutab Transpordiameti andmeid. Arvutatud tulemused on informatiivsed ja põhinevad
        vastuvõetud Mootorsõidukimaksu seadusel, mis hakkab kehtima 1. jaanuar 2025. Mootorsõidukimaks koosneb kahest
        osast: aastamaksust ja registreerimistasust. Registreerimistasu makstakse sõiduki registreerimisel
        liiklusregistrisse või sõiduki esimesel omanikuvahetusel. Aastamaksu tasutakse kaks korda aastas.
      </p>

      <div class="flex justify-center mb-4 border-b border-gray-300">
        <button class="px-4 py-2 text-sm"
          :class="tab === 'reg' ? 'text-blue-600 border-b-2 border-blue-600' : 'text-gray-600 hover:text-gray-800'"
          @click="tab = 'reg'">
          Registrinumber
        </button>
        <button class="px-4 py-2 text-sm"
          :class="tab === 'data' ? 'text-blue-600 border-b-2 border-blue-600' : 'text-gray-600 hover:text-gray-800'"
          @click="tab = 'data'">
          Sõiduki baasandmed
        </button>
      </div>

      <div v-if="tab === 'reg'" class="mb-6">
        <div class="flex items-center justify-center mb-4 w-full bg-gray-100 rounded-lg">
          <div class="bg-blue-600 text-white px-4 py-2 rounded-l">EU</div>
          <input v-model="regNumber" type="text" placeholder="123ABC"
            class="w-full p-3 outline-none bg-transparent text-gray-800 placeholder-gray-400 uppercase" maxlength="9" />
        </div>
      </div>

      <div v-else class="space-y-4 mb-6">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 text-left">
          <div>
            <label class="block font-medium mb-2 text-gray-700">Sõiduki tüüp</label>
            <select v-model="vehicleType" class="w-full border rounded-lg p-3">
              <option value=""></option>
              <option value="m1">Sõiduauto (M1)</option>
              <option value="n1">Kaubik/veoauto (N1)</option>
              <option value="l3e">Mootorratas/ATV (L3e-, L4e-, L5e-, L6e, L7e)</option>
              <option value="ms2">Ratasmaastikusõiduk (MS2)</option>
              <option value="t1b">Traktor (T1b, T3, T5)</option>
            </select>
          </div>
          <div>
            <label class="block font-medium mb-2 text-gray-700">Esmareg. aasta</label>
            <input v-model="firstRegYear" type="number" placeholder="Näiteks: 2019" min="1950"
              :max="new Date().getFullYear()" class="w-full border rounded-lg p-3" />
          </div>

          <template v-if="vehicleType === 'm1' || vehicleType === 'n1'">
            <div>
              <label class="block font-medium mb-2 text-gray-700">Mootoritüüp</label>
              <select v-model="engineType" class="w-full border rounded-lg p-3">
                <option value=""></option>
                <option value="tavaauto">Tavaauto</option>
                <option value="hübriid">Hübriid</option>
                <option value="pistikhübriid">Pistikhübriid</option>
                <option value="elektriauto">Elektriauto</option>
              </select>
            </div>
            <div v-if="engineType === 'tavaauto'">
              <label class="block font-medium mb-2 text-gray-700">CO2 standard</label>
              <select v-model="co2Standard" class="w-full border rounded-lg p-3">
                <option value=""></option>
                <option value="wltp">WLTP</option>
                <option value="nedc">NEDC</option>
                <option value="puudub">Puudub</option>
              </select>
            </div>
            <template v-if="engineType === 'tavaauto' && co2Standard === 'puudub'">
              <div>
                <label class="block font-medium mb-2 text-gray-700">Kütus</label>
                <select v-model="fuel" class="w-full border rounded-lg p-3">
                  <option value=""></option>
                  <option value="Petrol">Bensiin</option>
                  <option value="Diesel">Diisel</option>
                </select>
              </div>
              <div>
                <label class="block font-medium mb-2 text-gray-700">Mootori võimsus (kW)</label>
                <input v-model="power" type="number" class="w-full border rounded-lg p-3" />
              </div>
              <div>
                <label class="block font-medium mb-2 text-gray-700">Tühimass (kg)</label>
                <input v-model="emptyWeight" type="number" class="w-full border rounded-lg p-3" />
              </div>
              <div>
                <label class="block font-medium mb-2 text-gray-700">Täismass (kg)</label>
                <input v-model="grossWeight" type="number" class="w-full border rounded-lg p-3" />
              </div>
            </template>
            <div v-if="engineType === 'tavaauto' && (co2Standard === 'wltp' || co2Standard === 'nedc')">
              <label class="block font-medium mb-2 text-gray-700">CO2 (g/km)</label>
              <input v-model="co2" type="number" class="w-full border rounded-lg p-3" />
            </div>
            <div v-if="engineType === 'hübriid'">
              <label class="block font-medium mb-2 text-gray-700">CO2 standard</label>
              <select v-model="co2Standard" class="w-full border rounded-lg p-3">
                <option value=""></option>
                <option value="wltp">WLTP</option>
                <option value="nedc">NEDC</option>
                <option value="puudub">Puudub</option>
              </select>
            </div>
            <template v-if="engineType === 'hübriid' && co2Standard === 'puudub'">
              <div>
                <label class="block font-medium mb-2 text-gray-700">Kütus</label>
                <select v-model="fuel" class="w-full border rounded-lg p-3">
                  <option value=""></option>
                  <option value="Petrol">Bensiin</option>
                  <option value="Diesel">Diisel</option>
                </select>
              </div>
              <div>
                <label class="block font-medium mb-2 text-gray-700">Mootori võimsus (kW)</label>
                <input v-model="power" type="number" class="w-full border rounded-lg p-3" />
              </div>
              <div>
                <label class="block font-medium mb-2 text-gray-700">Tühimass (kg)</label>
                <input v-model="emptyWeight" type="number" class="w-full border rounded-lg p-3" />
              </div>
              <div>
                <label class="block font-medium mb-2 text-gray-700">Täismass (kg)</label>
                <input v-model="grossWeight" type="number" class="w-full border rounded-lg p-3" />
              </div>
            </template>
            <template v-if="engineType === 'hübriid' && (co2Standard === 'wltp' || co2Standard === 'nedc')">
              <div>
                <label class="block font-medium mb-2 text-gray-700">CO2 (g/km)</label>
                <input v-model="co2" type="number" class="w-full border rounded-lg p-3" />
              </div>
              <div>
                <label class="block font-medium mb-2 text-gray-700">Täismass (kg)</label>
                <input v-model="grossWeight" type="number" class="w-full border rounded-lg p-3" />
              </div>
            </template>
            <div v-if="engineType === 'pistikhübriid' || engineType === 'elektriauto'">
              <label class="block font-medium mb-2 text-gray-700">Täismass (kg)</label>
              <input v-model="grossWeight" type="number" class="w-full border rounded-lg p-3" />
            </div>
          </template>

          <template v-if="vehicleType === 'l3e' || vehicleType === 'ms2' || vehicleType === 't1b'">
            <div>
              <label class="block font-medium mb-2 text-gray-700">Töömaht (cm³)</label>
              <input v-model="engineVolume" type="number" class="w-full border rounded-lg p-3" />
            </div>
          </template>
        </div>
      </div>

      <button
        class="bg-yellow-400 hover:bg-yellow-500 text-white font-semibold px-6 py-3 mt-4 rounded-lg transition-colors duration-200 shadow-md hover:shadow-lg"
        :disabled="!canCalculate" :class="{ 'opacity-50 cursor-not-allowed': !canCalculate }" @click="calculate">
        Arvuta
      </button>
    </main>

    <div v-if="showResults"
      class="z-20 bg-white -mt-8 p-8 shadow-lg w-full max-w-[70%] mx-auto text-left rounded-b-lg">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mb-6">
        <div>
          <h3 class="text-xl font-semibold mb-2">Registreerimistasu <span class="float-right">{{ vehicleData.regFee }} €</span></h3>
          <p class="text-sm text-gray-700">
            Registreerimistasu makstakse sõiduki registreerimisel liiklusregistrisse või sõiduki esimesel
            omanikuvahetusel.
            Registreerimistasu koosneb baasosast, CO₂ näitajast ja massist. Tasu väheneb vastavalt sõiduki vanusele.
            Mida
            vanem sõiduk, seda väiksem tasu.
          </p>
        </div>
        <div>
          <h3 class="text-xl font-semibold mb-2">Aastamaks <span class="float-right">{{vehicleData.yearFee}} €</span></h3>
          <p class="text-sm text-gray-700">
            Aastamaksu tasutakse kaks korda aastas. Aastamaks koosneb baasosast, CO₂ ja massi komponendist.
            Mootorsõidukimaksust tasutakse 50 protsenti 15. juuniks ja 50 protsenti 15. detsembriks. Maks väheneb
            vastavalt
            sõiduki vanusele. Mida vanem sõiduk, seda väiksem maks.
          </p>
        </div>
      </div>

      <div class="max-h-[300px] border border-gray-300 rounded-lg">
        <table class="min-w-full text-sm border-separate border-spacing-0">
          <thead class="bg-blue-900 text-white">
            <tr>
              <th class="p-3 text-left border-b border-blue-800">Reg. nr</th>
              <th class="p-3 text-left border-b border-blue-800">Mark ja mudel</th>
              <th class="p-3 text-left border-b border-blue-800">Aasta</th>
              <th class="p-3 text-left border-b border-blue-800">Kütus</th>
              <th class="p-3 text-left border-b border-blue-800">Võimsus</th>
              <th class="p-3 text-left border-b border-blue-800">Täismass</th>
              <th class="p-3 text-left border-b border-blue-800">CO₂ norm</th>
              <th class="p-3 text-left border-b border-blue-800">CO₂ g/km</th>
              <th class="p-3 text-left border-b border-blue-800">Registreerimistasu</th>
              <th class="p-3 text-left border-b border-blue-800">Aastamaks</th>
            </tr>
          </thead>
          <tbody class="bg-gray-100">
            <tr>
              <td class="p-3 border-b border-gray-300">{{ vehicleData.regNr || '-' }}</td>
              <td class="p-3 border-b border-gray-300">{{ vehicleData.markAndModel || '-' }}</td>
              <td class="p-3 border-b border-gray-300">{{ vehicleData.year || '-' }}</td>
              <td class="p-3 border-b border-gray-300">{{ vehicleData.fuel || '-' }}</td>
              <td class="p-3 border-b border-gray-300">{{ vehicleData.power || '-' }}</td>
              <td class="p-3 border-b border-gray-300">{{ vehicleData.weight || '-' }}</td>
              <td class="p-3 border-b border-gray-300">{{ vehicleData.co2norm || '-' }}</td>
              <td class="p-3 border-b border-gray-300">{{ vehicleData.co2gkg || '-' }}</td>
              <td class="p-3 border-b border-gray-300">{{ vehicleData.regFee || '–' }} €</td>
              <td class="p-3 border-b border-gray-300">{{ vehicleData.yearFee || '–' }} €</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <footer class="z-20 absolute bottom-0 left-0 w-full text-xs text-black text-center bg-white bg-opacity-50 p-8 ">
      Copyright © 2025 Automaksukalkulaator
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import axios from 'axios'


const vehicleData = ref({
  regNr: '',
  markAndModel: '',
  year: '',
  fuel: '',
  power: '',
  weight: '',
  co2norm: '',
  co2gkg: '',
  regFee: '',
  yearFee: ''
})
const tab = ref('reg')
const regNumber = ref('')
const vehicleType = ref('')
const firstRegYear = ref('')
const engineType = ref('')
const co2Standard = ref('')
const co2 = ref('')
const fuel = ref('')
const power = ref('')
const emptyWeight = ref('')
const grossWeight = ref('')
const engineVolume = ref('')

const showResults = ref(false)

const canCalculate = computed(() => {
  if (tab.value === 'reg') {
    return regNumber.value.length >= 3
  } else {
    return vehicleType.value && firstRegYear.value
  }
})



const calculate = async () => {
  if (canCalculate.value) {
    showResults.value = true
    if( tab.value === 'reg') {
      try {
        const response = await axios.get(`https://automakskalkulaator.ee/wp-json/custom-api/v1/fetch-vehicle?regnr=${regNumber.value}`)
        const data = response.data;

        const tempDiv = document.createElement('div');
        tempDiv.innerHTML = response.data['history_vehicleContainer_html'];
        const row = tempDiv.querySelector('table tr:nth-of-type(2)');
        const getText = (selector) => row.querySelector(selector)?.textContent?.trim() ?? null;

        vehicleData.value = {
          regNr: getText('.regNr'),
          markAndModel: getText('.markAndModel'),
          year: parseInt(getText('.year')),
          fuel: getText('.fuel'),
          power: getText('.power'),
          weight: getText('.weight'),
          co2norm: getText('.co2norm'),
          co2gkg: parseInt(getText('.co2gkg')),
          regFee: parseFloat(getText('.regFee')?.replace(/[^\d,.-]/g, '').replace(',', '.')),
          yearFee: parseFloat(getText('.yearFee')?.replace(/[^\d,.-]/g, '').replace(',', '.')),
        };
      } catch(er) {
        console.error('Error fetching vehicle data:', er)
      }
    }
  }
}
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}
</style>