<script setup>
// HomeView component for Supply Chain Carbon Calculator
import { ref, onMounted } from 'vue'
import { RouterLink } from 'vue-router'

const isVisible = ref(false)

onMounted(() => {
  setTimeout(() => {
    isVisible.value = true
  }, 100)
})
</script>

<template>
  <div
    class="w-full flex flex-col gap-32 items-center bg-gradient-to-br from-emerald-50 via-teal-50 to-cyan-50 min-h-screen overflow-hidden"
  >
    <!-- Animated background elements -->
    <div class="fixed inset-0 pointer-events-none">
      <div
        class="absolute top-20 left-10 w-72 h-72 bg-emerald-200 rounded-full mix-blend-multiply filter blur-xl opacity-30 animate-float"
      ></div>
      <div
        class="absolute top-40 right-10 w-72 h-72 bg-teal-200 rounded-full mix-blend-multiply filter blur-xl opacity-30 animate-float-delayed"
      ></div>
      <div
        class="absolute bottom-20 left-1/2 w-72 h-72 bg-cyan-200 rounded-full mix-blend-multiply filter blur-xl opacity-30 animate-float-slow"
      ></div>
    </div>

    <!-- Hero Section -->
    <section
      class="w-full flex flex-col items-center justify-center text-center py-20 px-4 relative z-10"
    >
      <div class="space-y-6 max-w-4xl" :class="{ 'animate-fade-in-up': isVisible }">
        <h1
          class="text-5xl md:text-7xl font-black text-transparent bg-clip-text bg-gradient-to-r from-emerald-600 via-teal-600 to-cyan-600 leading-tight tracking-tight"
        >
          Supply Chain<br />
          <span class="text-emerald-800">Carbon Calculator</span>
        </h1>

        <p class="text-xl md:text-2xl text-slate-600 mb-10 max-w-3xl leading-relaxed font-medium">
          Transform your supply chain with AI-powered carbon footprint analysis.
          <span class="text-emerald-600 font-semibold">Measure, optimize, and lead</span>
          the sustainability revolution.
        </p>

        <div class="flex flex-col sm:flex-row gap-4 justify-center items-center">
          <RouterLink
            to="/calculator"
            class="group relative bg-gradient-to-r from-emerald-500 to-teal-500 text-white px-12 py-4 rounded-2xl text-lg font-semibold shadow-2xl hover:shadow-emerald-500/25 transform hover:scale-105 transition-all duration-300 overflow-hidden"
          >
            <div
              class="absolute inset-0 bg-gradient-to-r from-emerald-600 to-teal-600 opacity-0 group-hover:opacity-100 transition-opacity duration-300"
            ></div>
            <span class="relative z-10 flex items-center gap-2">
              Start Your Analysis
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
          </RouterLink>

          <button
            class="group bg-white/80 backdrop-blur-sm text-emerald-700 px-8 py-4 rounded-2xl text-lg font-semibold border-2 border-emerald-200 hover:border-emerald-300 hover:bg-white transition-all duration-300"
          >
            <span class="flex items-center gap-2">
              <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M14.828 14.828a4 4 0 01-5.656 0M9 10h1.5a1.5 1.5 0 011.5 1.5v1a1.5 1.5 0 01-1.5 1.5H9m0-4h1.5a1.5 1.5 0 011.5 1.5v1a1.5 1.5 0 01-1.5 1.5H9m0-4V8a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2zm0 0V6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2z"
                ></path>
              </svg>
              Watch Demo
            </span>
          </button>
        </div>

        <div class="flex items-center justify-center gap-4 pt-4">
          <div class="flex items-center gap-2 text-emerald-600">
            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
              <path
                fill-rule="evenodd"
                d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
                clip-rule="evenodd"
              ></path>
            </svg>
            <span class="text-sm font-medium">No signup required</span>
          </div>
          <div class="w-1 h-1 bg-emerald-300 rounded-full"></div>
          <div class="flex items-center gap-2 text-emerald-600">
            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
              <path
                fill-rule="evenodd"
                d="M5 9V7a5 5 0 0110 0v2a2 2 0 012 2v5a2 2 0 01-2 2H5a2 2 0 01-2-2v-5a2 2 0 012-2zm8-2v2H7V7a3 3 0 016 0z"
                clip-rule="evenodd"
              ></path>
            </svg>
            <span class="text-sm font-medium">100% secure</span>
          </div>
        </div>
      </div>
    </section>

    <!-- How It Works -->
    <section class="w-full max-w-6xl flex flex-col items-center gap-16 px-4 relative z-10">
      <div class="text-center">
        <h2 class="text-4xl md:text-5xl font-bold text-slate-800 mb-4">How It Works</h2>
        <p class="text-xl text-slate-600 max-w-2xl">Three simple steps to carbon clarity</p>
      </div>

      <div class="flex flex-col lg:flex-row gap-8 w-full justify-center">
        <div
          class="group relative flex-1 bg-white/70 backdrop-blur-sm rounded-3xl shadow-xl hover:shadow-2xl transition-all duration-500 transform hover:-translate-y-2 p-8 border border-white/20"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-emerald-50 to-teal-50 rounded-3xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10 flex flex-col items-center text-center">
            <div
              class="text-6xl mb-6 transform group-hover:scale-110 transition-transform duration-300"
            >
              📝
            </div>
            <div class="w-12 h-1 bg-emerald-400 rounded-full mb-4"></div>
            <h3 class="text-2xl font-bold text-slate-800 mb-3">Input Data</h3>
            <p class="text-slate-600 leading-relaxed">
              Upload your supply chain data or use our guided input wizard to get started in
              minutes.
            </p>
          </div>
        </div>

        <div
          class="group relative flex-1 bg-white/70 backdrop-blur-sm rounded-3xl shadow-xl hover:shadow-2xl transition-all duration-500 transform hover:-translate-y-2 p-8 border border-white/20"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-teal-50 to-cyan-50 rounded-3xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10 flex flex-col items-center text-center">
            <div
              class="text-6xl mb-6 transform group-hover:scale-110 transition-transform duration-300"
            >
              ⚡
            </div>
            <div class="w-12 h-1 bg-teal-400 rounded-full mb-4"></div>
            <h3 class="text-2xl font-bold text-slate-800 mb-3">AI Analysis</h3>
            <p class="text-slate-600 leading-relaxed">
              Our advanced algorithms analyze your data and generate comprehensive carbon footprint
              insights.
            </p>
          </div>
        </div>

        <div
          class="group relative flex-1 bg-white/70 backdrop-blur-sm rounded-3xl shadow-xl hover:shadow-2xl transition-all duration-500 transform hover:-translate-y-2 p-8 border border-white/20"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-cyan-50 to-blue-50 rounded-3xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10 flex flex-col items-center text-center">
            <div
              class="text-6xl mb-6 transform group-hover:scale-110 transition-transform duration-300"
            >
              🌱
            </div>
            <div class="w-12 h-1 bg-cyan-400 rounded-full mb-4"></div>
            <h3 class="text-2xl font-bold text-slate-800 mb-3">Optimize</h3>
            <p class="text-slate-600 leading-relaxed">
              Receive personalized recommendations and track your progress towards carbon
              neutrality.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- Features Section -->
    <section class="w-full max-w-7xl flex flex-col items-center gap-16 px-4 relative z-10">
      <div class="text-center">
        <h2 class="text-4xl md:text-5xl font-bold text-slate-800 mb-4">Why Choose Our Platform?</h2>
        <p class="text-xl text-slate-600 max-w-3xl">
          Built for modern businesses who care about their environmental impact
        </p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-4 gap-8 w-full">
        <div
          class="group relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-lg hover:shadow-xl transition-all duration-300 transform hover:-translate-y-1 p-6 border border-white/50"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-emerald-400/10 to-teal-400/10 rounded-2xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10">
            <div
              class="text-4xl mb-4 transform group-hover:scale-110 transition-transform duration-300"
            >
              📊
            </div>
            <h3 class="text-xl font-bold text-slate-800 mb-2">Comprehensive Analysis</h3>
            <p class="text-slate-600 text-sm leading-relaxed">
              Full lifecycle assessment covering procurement, manufacturing, transportation, and
              disposal stages.
            </p>
          </div>
        </div>

        <div
          class="group relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-lg hover:shadow-xl transition-all duration-300 transform hover:-translate-y-1 p-6 border border-white/50"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-teal-400/10 to-cyan-400/10 rounded-2xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10">
            <div
              class="text-4xl mb-4 transform group-hover:scale-110 transition-transform duration-300"
            >
              ⚙️
            </div>
            <h3 class="text-xl font-bold text-slate-800 mb-2">Intuitive Interface</h3>
            <p class="text-slate-600 text-sm leading-relaxed">
              Clean, user-friendly design that makes complex carbon calculations accessible to
              everyone.
            </p>
          </div>
        </div>

        <div
          class="group relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-lg hover:shadow-xl transition-all duration-300 transform hover:-translate-y-1 p-6 border border-white/50"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-cyan-400/10 to-blue-400/10 rounded-2xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10">
            <div
              class="text-4xl mb-4 transform group-hover:scale-110 transition-transform duration-300"
            >
              📈
            </div>
            <h3 class="text-xl font-bold text-slate-800 mb-2">Real-time Insights</h3>
            <p class="text-slate-600 text-sm leading-relaxed">
              Dynamic dashboards with interactive charts and actionable sustainability metrics.
            </p>
          </div>
        </div>

        <div
          class="group relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-lg hover:shadow-xl transition-all duration-300 transform hover:-translate-y-1 p-6 border border-white/50"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-blue-400/10 to-indigo-400/10 rounded-2xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10">
            <div
              class="text-4xl mb-4 transform group-hover:scale-110 transition-transform duration-300"
            >
              🔒
            </div>
            <h3 class="text-xl font-bold text-slate-800 mb-2">Enterprise Security</h3>
            <p class="text-slate-600 text-sm leading-relaxed">
              Bank-grade encryption and privacy protection. Your sensitive data never leaves your
              control.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA Section -->
    <section class="w-full flex flex-col items-center justify-center py-16 px-4 relative z-10">
      <div class="relative max-w-4xl w-full">
        <div
          class="absolute inset-0 bg-gradient-to-r from-emerald-400 to-teal-400 rounded-3xl blur-xl opacity-20"
        ></div>
        <div
          class="relative bg-white/90 backdrop-blur-sm rounded-3xl shadow-2xl p-12 border border-white/50"
        >
          <div class="text-center space-y-6">
            <h2 class="text-3xl md:text-4xl font-bold text-slate-800">
              Ready to Transform Your Supply Chain?
            </h2>
            <p class="text-xl text-slate-600 max-w-2xl mx-auto">
              Join thousands of companies already reducing their carbon footprint with our platform.
            </p>

            <div class="flex flex-col sm:flex-row gap-4 justify-center items-center pt-4">
              <RouterLink
                to="/calculator"
                class="group relative bg-gradient-to-r from-emerald-500 to-teal-500 text-white px-10 py-4 rounded-2xl text-lg font-semibold shadow-xl hover:shadow-emerald-500/25 transform hover:scale-105 transition-all duration-300"
              >
                <span class="flex items-center gap-2">
                  Get Started Free
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
              </RouterLink>

              <div class="flex items-center gap-2 text-emerald-600">
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                  <path
                    fill-rule="evenodd"
                    d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
                    clip-rule="evenodd"
                  ></path>
                </svg>
                <span class="text-sm font-medium">Complete analysis in under 2 minutes</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer spacer -->
    <div class="h-16"></div>
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

@keyframes fade-in-up {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
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

.animate-fade-in-up {
  animation: fade-in-up 0.8s ease-out forwards;
}
</style>
