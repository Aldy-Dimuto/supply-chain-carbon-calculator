<script setup>
// AboutView component for Supply Chain Carbon Calculator
import { ref, onMounted } from 'vue'

const isVisible = ref(false)

onMounted(() => {
  setTimeout(() => {
    isVisible.value = true
  }, 200)
})
</script>

<template>
  <div
    class="w-full flex flex-col gap-32 items-center min-h-screen bg-gradient-to-br from-emerald-50 via-teal-50 to-cyan-50 overflow-hidden"
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
          class="text-4xl md:text-6xl font-black text-transparent bg-clip-text bg-gradient-to-r from-teal-600 via-cyan-600 to-blue-600 leading-tight tracking-tight"
        >
          About Our
          <span class="text-emerald-800 block">Carbon Calculator</span>
        </h1>

        <p class="text-xl md:text-2xl text-slate-600 mb-8 max-w-3xl leading-relaxed font-medium">
          We're on a mission to make
          <span class="text-teal-600 font-semibold">sustainability accessible</span>
          to every business, regardless of size or industry.
        </p>
      </div>
    </section>

    <!-- Mission Section -->
    <section class="w-full max-w-4xl px-4 relative z-10">
      <div
        class="group relative bg-white/80 backdrop-blur-sm rounded-3xl shadow-2xl hover:shadow-3xl transition-all duration-500 transform hover:-translate-y-1 p-12 border border-white/50"
      >
        <div
          class="absolute inset-0 bg-gradient-to-br from-emerald-50 to-teal-50 rounded-3xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
        ></div>
        <div class="relative z-10 flex flex-col items-center text-center">
          <div
            class="text-6xl mb-6 transform group-hover:scale-110 transition-transform duration-300"
          >
            🌍
          </div>
          <div
            class="w-16 h-1 bg-gradient-to-r from-emerald-400 to-teal-400 rounded-full mb-6"
          ></div>
          <h2 class="text-3xl md:text-4xl font-bold text-slate-800 mb-6">Our Mission</h2>
          <p class="text-xl text-slate-600 leading-relaxed max-w-2xl">
            We believe every business can make a positive impact on our planet. Our platform
            empowers you to
            <span class="text-emerald-600 font-semibold">understand, measure, and reduce</span> your
            supply chain's environmental footprint with precision and ease.
          </p>
        </div>
      </div>
    </section>

    <!-- Values Section -->
    <section class="w-full max-w-6xl flex flex-col items-center gap-16 px-4 relative z-10">
      <div class="text-center">
        <h2 class="text-3xl md:text-4xl font-bold text-slate-800 mb-4">What Drives Us</h2>
        <p class="text-xl text-slate-600 max-w-2xl">
          The principles that guide our work and shape our impact
        </p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-8 w-full">
        <div
          class="group relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-lg hover:shadow-2xl transition-all duration-500 transform hover:-translate-y-2 p-8 border border-white/50"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-emerald-400/10 to-teal-400/10 rounded-2xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10 flex flex-col items-center text-center">
            <div
              class="text-5xl mb-6 transform group-hover:scale-110 transition-transform duration-300"
            >
              🤝
            </div>
            <div class="w-12 h-1 bg-emerald-400 rounded-full mb-4"></div>
            <h3 class="text-2xl font-bold text-slate-800 mb-4">Collaboration</h3>
            <p class="text-slate-600 leading-relaxed">
              We partner with businesses, researchers, and sustainability experts to create
              solutions that drive real, measurable change across industries.
            </p>
          </div>
        </div>

        <div
          class="group relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-lg hover:shadow-2xl transition-all duration-500 transform hover:-translate-y-2 p-8 border border-white/50"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-teal-400/10 to-cyan-400/10 rounded-2xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10 flex flex-col items-center text-center">
            <div
              class="text-5xl mb-6 transform group-hover:scale-110 transition-transform duration-300"
            >
              🔬
            </div>
            <div class="w-12 h-1 bg-teal-400 rounded-full mb-4"></div>
            <h3 class="text-2xl font-bold text-slate-800 mb-4">Scientific Rigor</h3>
            <p class="text-slate-600 leading-relaxed">
              Our calculations are built on the latest climate science, peer-reviewed methodologies,
              and industry best practices for maximum accuracy.
            </p>
          </div>
        </div>

        <div
          class="group relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-lg hover:shadow-2xl transition-all duration-500 transform hover:-translate-y-2 p-8 border border-white/50"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-cyan-400/10 to-blue-400/10 rounded-2xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          ></div>
          <div class="relative z-10 flex flex-col items-center text-center">
            <div
              class="text-5xl mb-6 transform group-hover:scale-110 transition-transform duration-300"
            >
              💡
            </div>
            <div class="w-12 h-1 bg-cyan-400 rounded-full mb-4"></div>
            <h3 class="text-2xl font-bold text-slate-800 mb-4">Simplicity</h3>
            <p class="text-slate-600 leading-relaxed">
              We make complex sustainability metrics accessible to everyone, transforming intricate
              data into clear, actionable insights.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- Stats Section -->
    <section class="w-full max-w-5xl px-4 relative z-10">
      <div
        class="bg-gradient-to-r from-emerald-500 to-teal-500 rounded-3xl p-12 text-white shadow-2xl"
      >
        <div class="text-center mb-12">
          <h2 class="text-3xl md:text-4xl font-bold mb-4">Our Impact So Far</h2>
          <p class="text-xl text-emerald-100 max-w-2xl mx-auto">
            Together, we're making a difference across supply chains worldwide
          </p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <div class="text-center">
            <div class="text-4xl md:text-5xl font-black mb-2">1,250+</div>
            <div class="text-emerald-100 font-medium">Companies Analyzed</div>
          </div>
          <div class="text-center">
            <div class="text-4xl md:text-5xl font-black mb-2">15M+</div>
            <div class="text-emerald-100 font-medium">Tons CO₂ Tracked</div>
          </div>
          <div class="text-center">
            <div class="text-4xl md:text-5xl font-black mb-2">2.3M+</div>
            <div class="text-emerald-100 font-medium">Tons CO₂ Reduced</div>
          </div>
        </div>
      </div>
    </section>

    <!-- Team Section -->
    <section class="w-full max-w-6xl px-4 relative z-10">
      <div class="text-center mb-16">
        <h2 class="text-3xl md:text-4xl font-bold text-slate-800 mb-4">Built by Experts</h2>
        <p class="text-xl text-slate-600 max-w-3xl mx-auto">
          Our team combines decades of experience in sustainability, data science, and enterprise
          software
        </p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
        <div
          class="group relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-lg hover:shadow-xl transition-all duration-300 transform hover:-translate-y-1 p-6 border border-white/50 text-center"
        >
          <div
            class="w-16 h-16 bg-gradient-to-r from-emerald-400 to-teal-400 rounded-full mx-auto mb-4 flex items-center justify-center"
          >
            <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M13 10V3L4 14h7v7l9-11h-7z"
              ></path>
            </svg>
          </div>
          <h3 class="text-lg font-bold text-slate-800 mb-2">Climate Scientists</h3>
          <p class="text-slate-600 text-sm">
            PhD-level expertise in carbon accounting and lifecycle assessment
          </p>
        </div>

        <div
          class="group relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-lg hover:shadow-xl transition-all duration-300 transform hover:-translate-y-1 p-6 border border-white/50 text-center"
        >
          <div
            class="w-16 h-16 bg-gradient-to-r from-teal-400 to-cyan-400 rounded-full mx-auto mb-4 flex items-center justify-center"
          >
            <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2zm0 0V6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2z"
              ></path>
            </svg>
          </div>
          <h3 class="text-lg font-bold text-slate-800 mb-2">Data Engineers</h3>
          <p class="text-slate-600 text-sm">
            Specialists in large-scale data processing and analytics
          </p>
        </div>

        <div
          class="group relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-lg hover:shadow-xl transition-all duration-300 transform hover:-translate-y-1 p-6 border border-white/50 text-center"
        >
          <div
            class="w-16 h-16 bg-gradient-to-r from-cyan-400 to-blue-400 rounded-full mx-auto mb-4 flex items-center justify-center"
          >
            <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"
              ></path>
            </svg>
          </div>
          <h3 class="text-lg font-bold text-slate-800 mb-2">Industry Veterans</h3>
          <p class="text-slate-600 text-sm">
            Former sustainability leads from Fortune 500 companies
          </p>
        </div>

        <div
          class="group relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-lg hover:shadow-xl transition-all duration-300 transform hover:-translate-y-1 p-6 border border-white/50 text-center"
        >
          <div
            class="w-16 h-16 bg-gradient-to-r from-blue-400 to-indigo-400 rounded-full mx-auto mb-4 flex items-center justify-center"
          >
            <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z"
              ></path>
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
              ></path>
            </svg>
          </div>
          <h3 class="text-lg font-bold text-slate-800 mb-2">UX Designers</h3>
          <p class="text-slate-600 text-sm">
            User experience specialists focused on accessibility and usability
          </p>
        </div>
      </div>
    </section>

    <!-- Contact Section -->
    <section class="w-full flex flex-col items-center justify-center py-16 px-4 relative z-10">
      <div class="relative max-w-3xl w-full">
        <div
          class="absolute inset-0 bg-gradient-to-r from-teal-400 to-cyan-400 rounded-3xl blur-xl opacity-20"
        ></div>
        <div
          class="relative bg-white/90 backdrop-blur-sm rounded-3xl shadow-2xl p-12 border border-white/50"
        >
          <div class="text-center space-y-6">
            <div class="text-5xl mb-4">💬</div>
            <h2 class="text-3xl md:text-4xl font-bold text-slate-800">Let's Connect</h2>
            <p class="text-xl text-slate-600 max-w-2xl mx-auto">
              Have questions about our methodology? Want to discuss a partnership? We'd love to hear
              from you.
            </p>

            <div class="flex flex-col sm:flex-row gap-4 justify-center items-center pt-4">
              <a
                href="mailto:hello@supplychaincarbon.com"
                class="group relative bg-gradient-to-r from-teal-500 to-cyan-500 text-white px-10 py-4 rounded-2xl text-lg font-semibold shadow-xl hover:shadow-teal-500/25 transform hover:scale-105 transition-all duration-300"
              >
                <span class="flex items-center gap-2">
                  <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M3 8l7.89 4.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
                    ></path>
                  </svg>
                  Get in Touch
                </span>
              </a>

              <a
                href="https://calendly.com/supplychaincarbon"
                class="group bg-white/80 backdrop-blur-sm text-teal-700 px-8 py-4 rounded-2xl text-lg font-semibold border-2 border-teal-200 hover:border-teal-300 hover:bg-white transition-all duration-300"
              >
                <span class="flex items-center gap-2">
                  <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v14a2 2 0 002 2z"
                    ></path>
                  </svg>
                  Schedule a Call
                </span>
              </a>
            </div>

            <div class="flex items-center justify-center gap-4 pt-6">
              <div class="flex items-center gap-2 text-teal-600">
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                  <path
                    fill-rule="evenodd"
                    d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
                    clip-rule="evenodd"
                  ></path>
                </svg>
                <span class="text-sm font-medium">Response within 24 hours</span>
              </div>
              <div class="w-1 h-1 bg-teal-300 rounded-full"></div>
              <div class="flex items-center gap-2 text-teal-600">
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                  <path
                    fill-rule="evenodd"
                    d="M5 9V7a5 5 0 0110 0v2a2 2 0 012 2v5a2 2 0 01-2 2H5a2 2 0 01-2-2v-5a2 2 0 012-2zm8-2v2H7V7a3 3 0 016 0z"
                    clip-rule="evenodd"
                  ></path>
                </svg>
                <span class="text-sm font-medium">100% confidential</span>
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
