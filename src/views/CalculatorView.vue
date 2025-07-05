<script setup>
import { ref, computed } from 'vue'
import ResultsDashboard from './ResultsDashboard.vue'

const step = ref(1)
const maxStep = 5

// Form state
const product = ref({ type: '', weight: '', quantity: '' })
const raw = ref({ country: '', material: '' })
const manufacturing = ref({ location: '', energy: '', method: '' })
const transport = ref({ mode: '', distance: '', fuel: '' })

const goNext = () => {
  if (step.value < maxStep) step.value++
}
const goBack = () => {
  if (step.value > 1) step.value--
}
const startOver = () => {
  step.value = 1
  product.value = { type: '', weight: '', quantity: '' }
  raw.value = { country: '', material: '' }
  manufacturing.value = { location: '', energy: '', method: '' }
  transport.value = { mode: '', distance: '', fuel: '' }
}

// Emission factors
const rawFactors = {
  Steel: 2.5,
  Plastic: 3.0,
  Cotton: 5.0,
}
const manufacturingFactors = {
  Coal: 2.0,
  'Natural gas': 1.2,
  Renewable: 0.1,
}
const transportFactors = {
  Truck: 0.1,
  Ship: 0.02,
  Plane: 0.5,
}

const result = computed(() => {
  const w = parseFloat(product.value.weight) || 0
  const q = parseInt(product.value.quantity) || 0
  const mat = raw.value.material
  const matFactor = rawFactors[mat] || 0
  const energy = manufacturing.value.energy
  const manFactor = manufacturingFactors[energy] || 0
  const mode = transport.value.mode
  const transFactor = transportFactors[mode] || 0
  const d = parseFloat(transport.value.distance) || 0

  const rawEmissions = w * q * matFactor
  const manEmissions = w * q * manFactor
  // Enhanced transport calculation
  const transEmissions = d * (w * q) * transFactor
  const totalCO2 = rawEmissions + manEmissions + transEmissions

  // Percentage breakdown
  const breakdown = [
    { label: 'Raw Materials', value: rawEmissions },
    { label: 'Manufacturing', value: manEmissions },
    { label: 'Transport', value: transEmissions },
  ]
  breakdown.forEach((b) => {
    b.percent = totalCO2 > 0 ? ((b.value / totalCO2) * 100).toFixed(1) : '0.0'
  })

  // Industry average (for demo, 20% higher than calculated)
  const industryAverage = (totalCO2 * 1.2).toFixed(2)

  // Improvement scenario: switching to Ship if not already Ship
  let improvement = null
  if (mode !== 'Ship' && w * q > 0 && d > 0) {
    const shipEmissions = d * (w * q) * transportFactors['Ship']
    const saved = transEmissions - shipEmissions
    if (saved > 0.1) {
      improvement = `Switching to Ship would save ${saved.toFixed(2)} kg CO₂`
    }
  }

  return {
    totalCO2: totalCO2.toFixed(2),
    breakdown,
    suggestions: getSuggestions(breakdown),
    industryAverage,
    improvement,
  }
})

function getSuggestions(breakdown) {
  const total = breakdown.reduce((sum, b) => sum + b.value, 0)
  const suggestions = []
  breakdown.forEach((b) => {
    const pct = total > 0 ? b.value / total : 0
    if (b.label === 'Raw Materials' && pct > 0.4) {
      suggestions.push('Consider recycled materials or bio-based alternatives')
    }
    if (b.label === 'Manufacturing' && pct > 0.4) {
      suggestions.push('Switch to renewable energy or improve efficiency')
    }
    if (b.label === 'Transport' && pct > 0.4) {
      suggestions.push('Optimize logistics or use lower-carbon transport')
    }
  })
  return suggestions
}
</script>

<template>
  <div
    class="w-full min-h-screen flex flex-col items-center bg-gradient-to-br from-emerald-50 via-teal-50 to-cyan-50 py-12 px-4 overflow-hidden"
  >
    <!-- Animated background elements -->
    <div class="fixed inset-0 pointer-events-none">
      <div
        class="absolute top-20 left-10 w-72 h-72 bg-emerald-200 rounded-full mix-blend-multiply filter blur-xl opacity-20 animate-float"
      ></div>
      <div
        class="absolute top-40 right-10 w-72 h-72 bg-teal-200 rounded-full mix-blend-multiply filter blur-xl opacity-20 animate-float-delayed"
      ></div>
      <div
        class="absolute bottom-20 left-1/2 w-72 h-72 bg-cyan-200 rounded-full mix-blend-multiply filter blur-xl opacity-20 animate-float-slow"
      ></div>
    </div>

    <!-- Title -->
    <div class="relative z-10 text-center mb-8">
      <h1
        class="text-4xl md:text-5xl font-black text-transparent bg-clip-text bg-gradient-to-r from-emerald-600 via-teal-600 to-cyan-600 mb-4"
      >
        Carbon Calculator
      </h1>
      <p class="text-lg text-slate-600 max-w-2xl">
        Calculate your supply chain's carbon footprint in just a few simple steps
      </p>
    </div>

    <!-- Progress Stepper -->
    <div class="relative z-10 w-full max-w-4xl mb-12">
      <div class="flex justify-between items-center">
        <div v-for="i in maxStep" :key="i" class="flex flex-col items-center flex-1">
          <div class="flex items-center w-full">
            <div
              class="w-8 h-8 rounded-full flex items-center justify-center text-sm font-bold transition-all duration-300"
              :class="
                step >= i
                  ? 'bg-gradient-to-r from-emerald-500 to-teal-500 text-white shadow-lg'
                  : 'bg-white/70 text-slate-400 border-2 border-slate-200'
              "
            >
              {{ i }}
            </div>
            <div
              v-if="i < maxStep"
              class="flex-1 h-0.5 mx-4 transition-all duration-300"
              :class="step > i ? 'bg-gradient-to-r from-emerald-500 to-teal-500' : 'bg-slate-200'"
            ></div>
          </div>
          <div class="text-xs font-medium text-slate-600 mt-2 text-center">
            {{
              i === 1
                ? 'Product'
                : i === 2
                  ? 'Raw Materials'
                  : i === 3
                    ? 'Manufacturing'
                    : i === 4
                      ? 'Transport'
                      : 'Results'
            }}
          </div>
        </div>
      </div>
    </div>

    <!-- Step Cards -->
    <transition name="fade" mode="out-in">
      <div :key="step" class="relative z-10 w-full max-w-2xl mx-auto">
        <!-- Step 1: Product Details -->
        <div
          v-if="step === 1"
          class="group relative bg-white/80 backdrop-blur-sm rounded-3xl shadow-2xl hover:shadow-emerald-500/20 transition-all duration-500 p-8 border border-white/50"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-emerald-50/50 to-teal-50/50 rounded-3xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10">
            <div class="flex items-center gap-3 mb-6">
              <div class="text-3xl">📦</div>
              <h2 class="text-2xl font-bold text-emerald-700">Product Details</h2>
            </div>
            <div class="space-y-6">
              <div class="form-group">
                <label class="block text-sm font-semibold text-slate-700 mb-2">Product Type</label>
                <input
                  v-model="product.type"
                  type="text"
                  class="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-emerald-400 focus:outline-none transition-colors duration-200 bg-white/70 backdrop-blur-sm"
                  placeholder="e.g. T-shirt"
                />
              </div>
              <div class="form-group">
                <label class="block text-sm font-semibold text-slate-700 mb-2">Weight (kg)</label>
                <input
                  v-model="product.weight"
                  type="number"
                  min="0"
                  step="0.1"
                  class="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-emerald-400 focus:outline-none transition-colors duration-200 bg-white/70 backdrop-blur-sm"
                  placeholder="e.g. 0.5"
                />
              </div>
              <div class="form-group">
                <label class="block text-sm font-semibold text-slate-700 mb-2">Quantity</label>
                <input
                  v-model="product.quantity"
                  type="number"
                  min="1"
                  class="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-emerald-400 focus:outline-none transition-colors duration-200 bg-white/70 backdrop-blur-sm"
                  placeholder="e.g. 100"
                />
              </div>
            </div>
          </div>
        </div>

        <!-- Step 2: Raw Materials -->
        <div
          v-else-if="step === 2"
          class="group relative bg-white/80 backdrop-blur-sm rounded-3xl shadow-2xl hover:shadow-teal-500/20 transition-all duration-500 p-8 border border-white/50"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-teal-50/50 to-cyan-50/50 rounded-3xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10">
            <div class="flex items-center gap-3 mb-6">
              <div class="text-3xl">🏭</div>
              <h2 class="text-2xl font-bold text-teal-700">Raw Materials</h2>
            </div>
            <div class="space-y-6">
              <div class="form-group">
                <label class="block text-sm font-semibold text-slate-700 mb-2"
                  >Source Country</label
                >
                <input
                  v-model="raw.country"
                  type="text"
                  class="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-teal-400 focus:outline-none transition-colors duration-200 bg-white/70 backdrop-blur-sm"
                  placeholder="e.g. China"
                />
              </div>
              <div class="form-group">
                <label class="block text-sm font-semibold text-slate-700 mb-2">Material Type</label>
                <select
                  v-model="raw.material"
                  class="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-teal-400 focus:outline-none transition-colors duration-200 bg-white/70 backdrop-blur-sm"
                >
                  <option disabled value="">Select material</option>
                  <option>Steel</option>
                  <option>Plastic</option>
                  <option>Cotton</option>
                </select>
              </div>
            </div>
          </div>
        </div>

        <!-- Step 3: Manufacturing -->
        <div
          v-else-if="step === 3"
          class="group relative bg-white/80 backdrop-blur-sm rounded-3xl shadow-2xl hover:shadow-cyan-500/20 transition-all duration-500 p-8 border border-white/50"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-cyan-50/50 to-blue-50/50 rounded-3xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10">
            <div class="flex items-center gap-3 mb-6">
              <div class="text-3xl">⚙️</div>
              <h2 class="text-2xl font-bold text-cyan-700">Manufacturing</h2>
            </div>
            <div class="space-y-6">
              <div class="form-group">
                <label class="block text-sm font-semibold text-slate-700 mb-2">Location</label>
                <input
                  v-model="manufacturing.location"
                  type="text"
                  class="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-cyan-400 focus:outline-none transition-colors duration-200 bg-white/70 backdrop-blur-sm"
                  placeholder="e.g. Vietnam"
                />
              </div>
              <div class="form-group">
                <label class="block text-sm font-semibold text-slate-700 mb-2">Energy Source</label>
                <select
                  v-model="manufacturing.energy"
                  class="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-cyan-400 focus:outline-none transition-colors duration-200 bg-white/70 backdrop-blur-sm"
                >
                  <option disabled value="">Select energy source</option>
                  <option>Coal</option>
                  <option>Natural gas</option>
                  <option>Renewable</option>
                </select>
              </div>
              <div class="form-group">
                <label class="block text-sm font-semibold text-slate-700 mb-2"
                  >Production Method</label
                >
                <input
                  v-model="manufacturing.method"
                  type="text"
                  class="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-cyan-400 focus:outline-none transition-colors duration-200 bg-white/70 backdrop-blur-sm"
                  placeholder="e.g. Automated"
                />
              </div>
            </div>
          </div>
        </div>

        <!-- Step 4: Transportation -->
        <div
          v-else-if="step === 4"
          class="group relative bg-white/80 backdrop-blur-sm rounded-3xl shadow-2xl hover:shadow-blue-500/20 transition-all duration-500 p-8 border border-white/50"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-blue-50/50 to-indigo-50/50 rounded-3xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10">
            <div class="flex items-center gap-3 mb-6">
              <div class="text-3xl">🚚</div>
              <h2 class="text-2xl font-bold text-blue-700">Transportation</h2>
            </div>
            <div class="space-y-6">
              <div class="form-group">
                <label class="block text-sm font-semibold text-slate-700 mb-2"
                  >Transport Mode</label
                >
                <select
                  v-model="transport.mode"
                  class="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-blue-400 focus:outline-none transition-colors duration-200 bg-white/70 backdrop-blur-sm"
                >
                  <option disabled value="">Select mode</option>
                  <option>Truck</option>
                  <option>Ship</option>
                  <option>Plane</option>
                </select>
              </div>
              <div class="form-group">
                <label class="block text-sm font-semibold text-slate-700 mb-2">Distance (km)</label>
                <input
                  v-model="transport.distance"
                  type="number"
                  min="0"
                  class="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-blue-400 focus:outline-none transition-colors duration-200 bg-white/70 backdrop-blur-sm"
                  placeholder="e.g. 5000"
                />
              </div>
              <div class="form-group">
                <label class="block text-sm font-semibold text-slate-700 mb-2">Fuel Type</label>
                <input
                  v-model="transport.fuel"
                  type="text"
                  class="w-full px-4 py-3 rounded-xl border-2 border-slate-200 focus:border-blue-400 focus:outline-none transition-colors duration-200 bg-white/70 backdrop-blur-sm"
                  placeholder="e.g. Diesel"
                />
              </div>
            </div>
          </div>
        </div>

        <!-- Step 5: Results Dashboard -->
        <div v-else-if="step === 5">
          <div v-if="result && result.breakdown && result.breakdown.length">
            <ResultsDashboard
              :total="result.totalCO2"
              :breakdown="result.breakdown"
              :suggestions="result.suggestions"
              :industry-average="result.industryAverage"
              :improvement="result.improvement"
              @start-over="startOver"
            />
          </div>
          <div
            v-else
            class="relative bg-white/80 backdrop-blur-sm rounded-3xl shadow-2xl p-8 border border-white/50 text-center"
          >
            <div class="text-red-500 text-lg font-semibold">⚠️ No results to display</div>
            <p class="text-slate-600 mt-2">Please check your input and try again.</p>
          </div>
        </div>

        <!-- Navigation Buttons -->
        <div v-if="step < 5" class="flex justify-between items-center mt-8">
          <button
            class="group relative bg-white/80 backdrop-blur-sm text-slate-600 px-6 py-3 rounded-2xl font-semibold border-2 border-slate-200 hover:border-slate-300 hover:bg-white transition-all duration-300 disabled:opacity-50 disabled:cursor-not-allowed"
            :disabled="step === 1"
            @click="goBack"
          >
            <span class="flex items-center gap-2">
              <svg
                class="w-5 h-5 group-hover:-translate-x-1 transition-transform"
                fill="none"
                stroke="currentColor"
                viewBox="0 0 24 24"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M11 17l-5-5m0 0l5-5m-5 5h12"
                ></path>
              </svg>
              Back
            </span>
          </button>

          <button
            class="group relative bg-gradient-to-r from-emerald-500 to-teal-500 text-white px-8 py-3 rounded-2xl font-semibold shadow-xl hover:shadow-emerald-500/25 transform hover:scale-105 transition-all duration-300"
            @click="goNext"
          >
            <span class="flex items-center gap-2">
              Next
              <svg
                class="w-5 h-5 group-hover:translate-x-1 transition-transform"
                fill="none"
                stroke="currentColor"
                viewBox="0 0 24 24"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M13 7l5 5m0 0l-5 5m5-5H6"
                ></path>
              </svg>
            </span>
          </button>
        </div>
      </div>
    </transition>
  </div>
</template>

<style scoped>
@keyframes float {
  0%,
  100% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-20px);
  }
}

@keyframes float-delayed {
  0%,
  100% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-15px);
  }
}

@keyframes float-slow {
  0%,
  100% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-25px);
  }
}

.animate-float {
  animation: float 6s ease-in-out infinite;
}

.animate-float-delayed {
  animation: float-delayed 8s ease-in-out infinite;
}

.animate-float-slow {
  animation: float-slow 10s ease-in-out infinite;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
