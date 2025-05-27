<template>
  <div class="flex-col h-full w-full overflow-hidden flex items-center justify-center bg-cover bg-center">
    
    <img
      src="./assets/automaksukalkulaator.webp"
      alt="Background"
      class="absolute inset-0 w-full h-full object-cover z-0"
    />

   
      <!-- Header -->
      <header class="absolute top-0 left-0 right-0 p-4 flex justify-between items-start w-full">
        <img src="./assets/automakskalkulaator_logo.png" alt="Logo" class="h-10" />
        <div class="text-right text-sm mt-2 mr-2">
          <p>Automaks ei rakendu:</p>
          <ul class="list-disc list-inside text-xs">
            <li>Alarmsõidukid</li>
            <li>Diplomaatilised sõidukid</li>
            <li>Kaitsevaldkonna sõidukid</li>
          </ul>
        </div>
      </header>

      <!-- Calculator -->
      <main class="z-20 bg-white text-black p-8 rounded-xl shadow-lg w-full max-w-lg mx-4 text-center">
        <h1 class="text-2xl font-semibold mb-4">Automaksu kalkulaator</h1>
        <p class="text-sm mb-4">
          Automaksu kalkulaator kasutab Transpordiameti andmeid. Arvutatud tulemused on
          informatiivsed ja põhinevad vastuvõetud Mootorsõidukimaksu seadusel, mis hakkab
          kehtima 1. jaanuar 2025. Mootorsõidukimaks koosneb kahest osast: aastamaksust ja
          registreerimistasust. Registreerimistasu makstakse sõiduki registreerimisel
          liiklusregistrisse või sõiduki esimesel omanikuvahetusel. Aastamaksu tasutakse kaks korda aastas.
        </p>

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

      <!-- Footer -->
      <footer class="absolute bottom-2 text-xs text-black background-white bg-opacity-50 p-2 rounded">
        Copyright © 2025 Automaksukalkulaator
      </footer>
    </div>
  
</template>

<style>
html, body {
  margin: 0;
  padding: 0;
  overflow: hidden;
}
</style>
