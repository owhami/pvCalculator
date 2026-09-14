<template>
  <div class="min-h-screen bg-gradient-to-br from-slate-50 via-blue-50/20 to-slate-100 text-slate-800 font-sans antialiased py-10 px-4 sm:px-6 lg:px-8 relative">
    <main class="max-w-3xl mx-auto space-y-8">

      <!-- Header / Hero Section -->
      <div class="bg-white rounded-3xl shadow-sm border border-slate-200/80 p-6 md:p-8 flex flex-col md:flex-row md:items-center md:justify-between gap-6">
        <div class="space-y-2">
          <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-amber-50 border border-amber-200 text-amber-700 text-xs font-semibold tracking-wide uppercase">
            <svg class="w-4 h-4 text-amber-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m3.343-5.657l-.707-.707m12.728 12.728l-.707-.707m0-11.314l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"></path>
            </svg>
            Solar Engineering Tool
          </div>
          <h1 class="text-2xl md:text-3xl font-extrabold tracking-tight text-slate-900">
            Kalkulator Tilt Angel Panel Surya
          </h1>
          <p class="text-slate-600 text-sm">
            Masukkan parameter geografis dan radiasi lokasi Anda untuk mendapatkan hasil analisis data yang optimal.
          </p>
        </div>
      </div>

      <!-- Input Form Card -->
      <div class="bg-white rounded-3xl shadow-sm border border-slate-200/80 p-6 md:p-8">
        <h2 class="text-lg font-bold text-slate-900 mb-6 flex items-center gap-2">
          <svg class="w-5 h-5 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z"></path>
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"></path>
          </svg>
          Parameter Lokasi & Medan
        </h2>

        <form @submit.prevent="calculateMonthlyTilt" class="space-y-6">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="space-y-1.5">
  <label class="block text-xs font-semibold tracking-wider text-slate-600 uppercase">
    Garis Lintang / Latitude (ϕ dalam derajat):
  </label>
  <input v-model.number="form.latitude" type="number" step="any" required class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-slate-800 text-sm focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-600 transition-all" placeholder="E.g: -1.247016 (Gunakan minus untuk LS)" />
</div>

            <div class="space-y-1.5 relative">
  <div class="flex items-center justify-between">
    <label class="block text-xs font-semibold tracking-wider text-slate-600 uppercase">
      Ground Albedo (ρ):
    </label>
    <button
      type="button"
      @click="showAlbedoInfo = !showAlbedoInfo"
      class="flex items-center justify-center w-4 h-4 rounded-full bg-slate-300 hover:bg-blue-600 text-white text-[10px] font-bold leading-none transition-colors cursor-pointer"
      aria-label="Info nilai Ground Albedo"
    >
      i
    </button>
  </div>
  <input v-model.number="form.albedo" type="number" step="any" required class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-slate-800 text-sm focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-600 transition-all" placeholder="E.g: 0.2 (Tanah/Rumput)" />

  <!-- Backdrop to close on outside click -->
  <div v-if="showAlbedoInfo" class="fixed inset-0 z-10" @click="showAlbedoInfo = false"></div>

  <!-- Albedo Reference Popover -->
  <div v-if="showAlbedoInfo" class="absolute right-0 top-full mt-1 z-20 w-72 max-w-[85vw] bg-white border border-slate-200 rounded-xl shadow-xl p-4">
    <div class="flex items-center justify-between mb-2">
      <p class="text-xs font-bold text-slate-800 uppercase tracking-wide">Referensi Nilai Albedo</p>
      <button type="button" @click="showAlbedoInfo = false" class="text-slate-400 hover:text-slate-700 cursor-pointer">
        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
        </svg>
      </button>
    </div>
    <p class="text-[11px] text-slate-500 mb-3">Albedo (ρ) adalah seberapa besar permukaan tanah memantulkan cahaya matahari. Pilih nilai sesuai kondisi medan di lokasi panel:</p>
    <ul class="text-xs text-slate-700 divide-y divide-slate-100">
      <li v-for="item in albedoReference" :key="item.label" class="flex items-center justify-between py-1.5">
        <span>{{ item.label }}</span>
        <span class="font-semibold text-blue-700 bg-blue-50 rounded px-1.5 py-0.5">{{ item.value }}</span>
      </li>
    </ul>
  </div>
</div>

<div class="space-y-1.5">
  <label class="block text-xs font-semibold tracking-wider text-slate-600 uppercase">
    Radiasi Global Horizontal (H<sub>g</sub> / kWh/m²/hari):
  </label>
  <input v-model.number="form.hg" type="number" step="any" required class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-slate-800 text-sm focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-600 transition-all" placeholder="E.g: 4.596" />
</div>

<div class="space-y-1.5">
  <label class="block text-xs font-semibold tracking-wider text-slate-600 uppercase">
    Radiasi Tersebar Horizontal (H<sub>d</sub> / kWh/m²/hari):
  </label>
  <input v-model.number="form.hd" type="number" step="any" required class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-slate-800 text-sm focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-600 transition-all" placeholder="E.g: 2.346" />
</div>

            <div class="space-y-1.5">
              <label class="block text-xs font-semibold tracking-wider text-slate-600 uppercase">
                Kondisi Medan / Terrain:
              </label>
              <select v-model="form.terrainType" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-slate-800 text-sm focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-600 transition-all">
                <option value="flat">Rata (Flat Ground)</option>
                <option value="sloped">Miring (Sloped Terrain)</option>
              </select>
            </div>

            <div v-if="form.terrainType === 'sloped'" class="space-y-1.5 animate-fadeIn">
  <label class="block text-xs font-semibold tracking-wider text-slate-600 uppercase">
    Sudut Kemiringan Tanah (<i>s</i> dalam derajat):
  </label>
  <input v-model.number="form.terrainSlope" type="number" step="any" :required="form.terrainType === 'sloped'" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-slate-800 text-sm focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-600 transition-all" placeholder="E.g: 15" />
</div>
          </div>

          <button type="submit" class="w-full py-3 px-6 bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-700 hover:to-indigo-700 text-white font-semibold rounded-xl shadow-md shadow-blue-500/20 focus:outline-none focus:ring-2 focus:ring-blue-500/45 transition-all flex items-center justify-center gap-2">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"></path>
            </svg>
            Generate Hasil Analisis
          </button>
        </form>
      </div>

      <!-- Panel Spec Card (Optional, for energy estimate) -->
<div class="bg-white rounded-3xl shadow-sm border border-slate-200/80 p-6 md:p-8">
  <h2 class="text-lg font-bold text-slate-900 mb-1 flex items-center gap-2">
    <svg class="w-5 h-5 text-emerald-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path>
    </svg>
    Spesifikasi Panel Surya (Opsional)
  </h2>
  <p class="text-slate-500 text-xs mb-6">
    Gunakan jika ingin melihat estimasi energi listrik (kWh). Kosongkan jika belum tahu spesifikasinya.
  </p>

  <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
    <div class="space-y-1.5">
      <label class="block text-xs font-semibold tracking-wider text-slate-600 uppercase">
        Panjang Panel (mm):
      </label>
      <input v-model.number="form.panelLength" type="number" step="any" min="0" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-slate-800 text-sm focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-600 transition-all" placeholder="E.g: 2382" />
    </div>

    <div class="space-y-1.5">
      <label class="block text-xs font-semibold tracking-wider text-slate-600 uppercase">
        Lebar Panel (mm):
      </label>
      <input v-model.number="form.panelWidth" type="number" step="any" min="0" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-slate-800 text-sm focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-600 transition-all" placeholder="E.g: 1134" />
    </div>

    <div class="space-y-1.5">
      <label class="block text-xs font-semibold tracking-wider text-slate-600 uppercase">
        Efisiensi Panel (%):
      </label>
      <input v-model.number="form.panelEfficiency" type="number" step="any" min="0" max="100" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-slate-800 text-sm focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-600 transition-all" placeholder="E.g: 23.3" />
    </div>

    <div class="space-y-1.5">
      <label class="block text-xs font-semibold tracking-wider text-slate-600 uppercase">
        Performance Ratio (PR):
      </label>
      <input v-model.number="form.performanceRatio" type="number" step="any" min="0" max="1" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-slate-800 text-sm focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-600 transition-all" placeholder="E.g: 0.8" />
    </div>
  </div>

  <div v-if="form.panelLength && form.panelWidth" class="mt-4 text-xs text-emerald-700 bg-emerald-50 px-3 py-1.5 rounded-lg inline-block">
    Luas Panel Terhitung: <strong>{{ ((form.panelLength / 1000) * (form.panelWidth / 1000)).toFixed(2) }} m²</strong>
  </div>
</div>
    </main>

    <!-- Modal Popup with Backdrop Blur -->
    <div v-if="showModal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-900/40 backdrop-blur-sm">
      <div class="bg-white rounded-3xl shadow-2xl border border-slate-200 w-full max-w-4xl max-h-[90vh] flex flex-col overflow-hidden">

        <!-- Modal Header -->
        <div class="p-6 md:p-8 border-b border-slate-100 flex items-center justify-between bg-slate-50/50">
          <div>
            <h2 class="text-xl font-extrabold text-slate-900">Hasil Analisis Bulanan & Rekomendasi Konstruksi</h2>
            <p class="text-slate-500 text-sm mt-0.5">Kondisi Medan: <span class="font-semibold text-slate-700">{{ form.terrainType === 'sloped' ? `Miring (${form.terrainSlope || 0}°)` : 'Rata (Flat)' }}</span></p>
          </div>
          <button @click="showModal = false" class="p-2 text-slate-400 hover:text-slate-700 hover:bg-slate-100 rounded-full transition-colors cursor-pointer">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
            </svg>
          </button>
        </div>

        <!-- Modal Body Content -->
        <div class="p-6 md:p-8 overflow-y-auto space-y-6 flex-1">
          <!-- Summary Metrics Cards -->
          <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
            <div class="bg-slate-50 p-4 rounded-2xl border border-slate-200/80">
              <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">Rata-rata Sudut Optimal</p>
              <p class="text-2xl font-bold text-blue-600 mt-1">{{ averageTilt }}°</p>
            </div>
            <div class="bg-slate-50 p-4 rounded-2xl border border-slate-200/80">
              <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">Rata-rata Radiasi Maks (H<sub>T</sub>)</p>
              <p class="text-2xl font-bold text-emerald-600 mt-1">{{ averageMaxHt }} <span class="text-sm font-normal text-slate-500">kWh/m²</span></p>
            </div>
            <div class="bg-slate-50 p-4 rounded-2xl border border-slate-200/80">
              <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">Rekomendasi Arah Hadap</p>
              <p class="text-base font-bold text-slate-900 mt-1 truncate">{{ monthlyResults[0]?.facingDirection }}</p>
            </div>
          </div>

          <!-- Energy Estimate Cards (only when panel spec is filled) -->
          <div v-if="energyEstimateActive" class="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div class="bg-amber-50 p-4 rounded-2xl border border-amber-200/80">
              <p class="text-xs font-medium text-amber-700 uppercase tracking-wider">Rata-rata Estimasi Energi Harian</p>
              <p class="text-2xl font-bold text-amber-700 mt-1">{{ averageEnergyPerDay }} <span class="text-sm font-normal text-amber-600">kWh/hari</span></p>
            </div>
            <div class="bg-amber-50 p-4 rounded-2xl border border-amber-200/80">
              <p class="text-xs font-medium text-amber-700 uppercase tracking-wider">Estimasi Energi per Tahun</p>
              <p class="text-2xl font-bold text-amber-700 mt-1">{{ annualEnergyEstimate }} <span class="text-sm font-normal text-amber-600">kWh/tahun</span></p>
            </div>
          </div>

          <!-- Detailed Table -->
          <div class="border border-slate-200 rounded-2xl overflow-hidden">
            <table class="w-full text-left text-sm border-collapse">
              <thead>
                <tr class="bg-slate-50 text-slate-600 uppercase text-xs tracking-wider border-b border-slate-200">
                  <th class="py-3 px-4 font-semibold">Bulan</th>
                  <th class="py-3 px-4 font-semibold">Hari Ke (<i>n</i>)</th>
                  <th class="py-3 px-4 font-semibold">Saran Arah Hadap</th>
                  <th class="py-3 px-4 font-semibold">Sudut Optimal (β)</th>
                  <th class="py-3 px-4 font-semibold">Total Radiasi Maksimum (H<sub>T</sub>)</th>
                  <th v-if="energyEstimateActive" class="py-3 px-4 font-semibold">Estimasi Energi</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-slate-100">
                <tr v-for="res in monthlyResults" :key="res.month" class="hover:bg-slate-50/50 transition-colors">
                  <td class="py-3 px-4 font-semibold text-slate-900">{{ res.month }}</td>
                  <td class="py-3 px-4 text-slate-600">{{ res.n }}</td>
                  <td class="py-3 px-4">
                    <span class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-medium bg-emerald-50 text-emerald-700 border border-emerald-200/60">
                      {{ res.facingDirection }}
                    </span>
                  </td>
                  <td class="py-3 px-4">
                    <span class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-bold bg-blue-50 text-blue-700 border border-blue-200/60">
                      {{ res.bestTilt }}°
                    </span>
                  </td>
                  <td class="py-3 px-4 font-medium text-slate-700">{{ res.maxHt.toFixed(2) }} <span class="text-xs font-normal text-slate-400">kWh/m²/hari</span></td>
                  <td v-if="energyEstimateActive" class="py-3 px-4 font-medium text-amber-700">{{ res.energyPerDay.toFixed(2) }} <span class="text-xs font-normal text-slate-400">kWh/hari</span></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <!-- Modal Footer with Actions -->
        <div class="p-6 border-t border-slate-100 bg-slate-50/50 flex items-center justify-end gap-4">
          <button @click="copyResultsToClipboard" class="w-full sm:w-auto px-5 py-2.5 bg-slate-900 hover:bg-slate-800 text-white font-medium rounded-xl text-sm transition-all flex items-center justify-center gap-2 shadow-sm cursor-pointer">
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3"></path>
            </svg>
            {{ copyStatus ? 'Berhasil Disalin!' : 'Salin Data' }}
          </button>
        </div>

      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref, computed } from 'vue'

const form = reactive({
  latitude: null,
  albedo: null,
  hg: null,
  hd: null,
  terrainType: 'flat',
  terrainSlope: null,
  panelLength: null,
  panelWidth: null,
  panelEfficiency: null,
  performanceRatio: null
})

const monthlyResults = ref([])
const showModal = ref(false)
const copyStatus = ref(false)
const energyEstimateActive = ref(false)
const showAlbedoInfo = ref(false)

const albedoReference = [
  { label: 'Rumput / vegetasi hijau', value: '0.15 - 0.25' },
  { label: 'Tanah kering / gundul', value: '0.20 - 0.30' },
  { label: 'Pasir kering', value: '0.25 - 0.45' },
  { label: 'Aspal (sudah lama)', value: '0.10 - 0.15' },
  { label: 'Beton / cor (baru)', value: '0.25 - 0.35' },
  { label: 'Air (danau/laut, tenang)', value: '~0.07' },
  { label: 'Atap / genteng gelap', value: '0.10 - 0.20' },
  { label: 'Salju segar', value: '0.80 - 0.90' }
]

const averageTilt = computed(() => {
  if (monthlyResults.value.length === 0) return 0
  const sum = monthlyResults.value.reduce((acc, curr) => acc + curr.bestTilt, 0)
  return (sum / monthlyResults.value.length).toFixed(1)
})

const averageMaxHt = computed(() => {
  if (monthlyResults.value.length === 0) return 0
  const sum = monthlyResults.value.reduce((acc, curr) => acc + curr.maxHt, 0)
  return (sum / monthlyResults.value.length).toFixed(2)
})

const averageEnergyPerDay = computed(() => {
  if (!energyEstimateActive.value || monthlyResults.value.length === 0) return '0.00'
  const sum = monthlyResults.value.reduce((acc, curr) => acc + curr.energyPerDay, 0)
  return (sum / monthlyResults.value.length).toFixed(2)
})

const annualEnergyEstimate = computed(() => {
  if (!energyEstimateActive.value || monthlyResults.value.length === 0) return '0'
  const sum = monthlyResults.value.reduce((acc, curr) => acc + curr.energyPerDay * curr.days, 0)
  return sum.toFixed(0)
})

const toRad = (deg) => (deg * Math.PI) / 180

const monthsData = [
  { name: 'Januari', n: 1, days: 31 },
  { name: 'Februari', n: 32, days: 28 },
  { name: 'Maret', n: 60, days: 31 },
  { name: 'April', n: 91, days: 30 },
  { name: 'Mei', n: 121, days: 31 },
  { name: 'Juni', n: 152, days: 30 },
  { name: 'Juli', n: 182, days: 31 },
  { name: 'Agustus', n: 213, days: 31 },
  { name: 'September', n: 244, days: 30 },
  { name: 'Oktober', n: 274, days: 31 },
  { name: 'November', n: 305, days: 30 },
  { name: 'Desember', n: 335, days: 31 }
]

const calculateMonthlyTilt = () => {
  const phi = form.latitude
  const rho = form.albedo
  const Hg = form.hg
  const Hd = form.hd

  // Pastikan data terisi sebelum menghitung
  if (phi === null || rho === null || Hg === null || Hd === null) {
    alert('Mohon lengkapi semua data parameter terlebih dahulu!')
    return
  }

  const s = form.terrainType === 'sloped' ? (form.terrainSlope || 0) : 0
  const sRad = toRad(s)

  // Estimasi energi hanya aktif jika spesifikasi panel diisi lengkap dan valid
  energyEstimateActive.value = (
    form.panelLength !== null && form.panelLength > 0 &&
    form.panelWidth !== null && form.panelWidth > 0 &&
    form.panelEfficiency !== null && form.panelEfficiency > 0 &&
    form.performanceRatio !== null && form.performanceRatio > 0
  )
  const panelArea = (form.panelLength/1000) * (form.panelWidth/1000)
  const panelEfficiency = form.panelEfficiency
  const performanceRatio = form.performanceRatio

  let facingDirection = 'Menghadap Utara'
  if (phi > 0) {
    facingDirection = 'Menghadap Selatan'
  } else if (phi < 0) {
    facingDirection = 'Menghadap Utara'
  } else {
    facingDirection = 'Datar / Fleksibel'
  }

  const results = []

  monthsData.forEach((m) => {
    let bestTilt = 0
    let maxHt = -1

    for (let beta = 0; beta <= 90; beta++) {
      const betaRad = toRad(beta)
      const delta = 23.45 * Math.sin(toRad((360 / 365) * (284 + m.n)))
      const deltaRad = toRad(delta)
      const phiRad = toRad(phi)
      const omega = 0

      const cosThetaZ = Math.cos(phiRad) * Math.cos(deltaRad) * Math.cos(omega) + Math.sin(phiRad) * Math.sin(deltaRad)

      const gammaRad = 0
      const cosTheta = Math.sin(deltaRad) * Math.sin(phiRad) * Math.cos(betaRad) -
                       Math.sin(deltaRad) * Math.cos(phiRad) * Math.sin(betaRad) * Math.cos(gammaRad) +
                       Math.cos(deltaRad) * Math.cos(phiRad) * Math.cos(betaRad) * Math.cos(omega) +
                       Math.cos(deltaRad) * Math.sin(phiRad) * Math.sin(betaRad) * Math.sin(gammaRad) * Math.sin(omega)

      let Rb = (cosTheta > 0 && cosThetaZ > 0) ? (cosTheta / cosThetaZ) : 0
      if (Rb < 0) Rb = 0

      const HB = (Hg - Hd) * Rb
      const Rd = (1 + Math.cos(betaRad + sRad)) / 2
      const HD = Rd * Hd
      const HR = Hg * rho * ((1 - Math.cos(betaRad + sRad)) / 2)
      const HT = HB + HD + HR

      if (HT > maxHt) {
        maxHt = HT
        bestTilt = beta
      }
    }

    // Estimasi energi keluaran (kWh/hari) = HT x Luas Panel x Efisiensi x Performance Ratio
    const energyPerDay = energyEstimateActive.value
      ? maxHt * panelArea * (panelEfficiency / 100) * performanceRatio
      : 0

    results.push({
      month: m.name,
      n: m.n,
      days: m.days,
      facingDirection,
      bestTilt,
      maxHt,
      energyPerDay
    })
  })

  monthlyResults.value = results
  showModal.value = true 
}

const copyResultsToClipboard = () => {
  let text = `=== HASIL ANALISIS SUDUT & ARAH HADAP PANEL SURYA ===\n`
  text += `Kondisi Medan: ${form.terrainType === 'sloped' ? 'Miring (' + (form.terrainSlope || 0) + '°)' : 'Rata (Flat)'}\n`
  text += `Rata-rata Sudut Optimal: ${averageTilt.value}°\n`
  text += `Rata-rata Radiasi Maksimum: ${averageMaxHt.value} kWh/m²/hari\n`
  text += `Rekomendasi Arah Hadap: ${monthlyResults.value[0]?.facingDirection}\n`
  if (energyEstimateActive.value) {
    text += `Rata-rata Estimasi Energi Harian: ${averageEnergyPerDay.value} kWh/hari\n`
    text += `Estimasi Energi per Tahun: ${annualEnergyEstimate.value} kWh/tahun\n`
  }
  text += `\n`

  const header = ['Bulan', 'Hari Ke (n)', 'Saran Arah Hadap', 'Sudut Optimal (deg)', 'Total Radiasi Maksimum (kWh/m2/hari)']
  if (energyEstimateActive.value) header.push('Estimasi Energi (kWh/hari)')
  text += header.join('\t') + '\n'

  monthlyResults.value.forEach(r => {
    const row = [r.month, r.n, r.facingDirection, r.bestTilt, r.maxHt.toFixed(2)]
    if (energyEstimateActive.value) row.push(r.energyPerDay.toFixed(2))
    text += row.join('\t') + '\n'
  })

  navigator.clipboard.writeText(text).then(() => {
    copyStatus.value = true
    setTimeout(() => {
      copyStatus.value = false
    }, 2000)
  })
}
</script>