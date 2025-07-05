<script setup>
import { ref, onMounted, watch } from 'vue'
import { Pie, Bar } from 'vue-chartjs'
import {
  Chart,
  Title,
  Tooltip,
  Legend,
  ArcElement,
  BarElement,
  CategoryScale,
  LinearScale,
} from 'chart.js'
import jsPDF from 'jspdf'

Chart.register(Title, Tooltip, Legend, ArcElement, BarElement, CategoryScale, LinearScale)

const props = defineProps({
  total: { type: String, default: '0' },
  breakdown: { type: Array, default: () => [] },
  suggestions: { type: Array, default: () => [] },
  industryAverage: { type: String, default: '' },
  improvement: { type: String, default: '' },
})

const isVisible = ref(false)

// Initialize with proper empty chart data structure
const pieData = ref({
  labels: [],
  datasets: [],
})
const barData = ref({
  labels: [],
  datasets: [],
})

const pieOptions = ref({
  responsive: true,
  plugins: {
    legend: {
      display: true,
      position: 'bottom',
      labels: {
        color: '#059669',
        font: { size: 14, weight: 'bold' },
      },
    },
    tooltip: {
      backgroundColor: 'rgba(255, 255, 255, 0.95)',
      titleColor: '#059669',
      bodyColor: '#475569',
      borderColor: '#10b981',
      borderWidth: 1,
      cornerRadius: 12,
      displayColors: false,
      callbacks: {
        label: function (context) {
          return context.label + ': ' + context.parsed + ' kg CO₂'
        },
      },
    },
  },
  animation: { animateScale: true },
})

const barOptions = ref({
  responsive: true,
  plugins: {
    legend: { display: false },
    tooltip: {
      backgroundColor: 'rgba(255, 255, 255, 0.95)',
      titleColor: '#059669',
      bodyColor: '#475569',
      borderColor: '#10b981',
      borderWidth: 1,
      cornerRadius: 12,
      displayColors: false,
      callbacks: {
        label: function (context) {
          return context.parsed.y + ' kg CO₂'
        },
      },
    },
  },
  scales: {
    x: {
      grid: { display: false },
      ticks: { color: '#059669', font: { weight: 'bold' } },
    },
    y: {
      grid: { color: '#d1fae5' },
      ticks: { color: '#059669' },
    },
  },
  animation: { duration: 800 },
})

function updateCharts() {
  const breakdown = Array.isArray(props.breakdown) ? props.breakdown : []

  // Only update if we have valid breakdown data
  if (breakdown.length === 0) {
    pieData.value = {
      labels: [],
      datasets: [],
    }
    barData.value = {
      labels: [],
      datasets: [],
    }
    return
  }

  const labels = breakdown.map((b) => b.label)
  const values = breakdown.map((b) => Number(b.value))

  pieData.value = {
    labels,
    datasets: [
      {
        data: values,
        backgroundColor: [
          'rgba(16, 185, 129, 0.8)',
          'rgba(45, 212, 191, 0.8)',
          'rgba(34, 211, 238, 0.8)',
        ],
        borderWidth: 2,
        borderColor: '#fff',
      },
    ],
  }

  barData.value = {
    labels,
    datasets: [
      {
        data: values,
        backgroundColor: 'rgba(16, 185, 129, 0.7)',
        borderRadius: 12,
        barPercentage: 0.6,
      },
    ],
  }
}

onMounted(() => {
  updateCharts()
  setTimeout(() => {
    isVisible.value = true
  }, 100)
})

watch(() => props.breakdown, updateCharts, { deep: true })

const pdfContent = ref(null)

// Fixed exportPDF function for your Vue component

async function exportPDF() {
  const doc = new jsPDF('portrait', 'mm', 'a4')
  const pageWidth = doc.internal.pageSize.getWidth()
  const pageHeight = doc.internal.pageSize.getHeight()
  const margin = 20
  const contentWidth = pageWidth - margin * 2

  let yPosition = margin

  // Helper functions
  function checkPageBreak(requiredHeight) {
    if (yPosition + requiredHeight > pageHeight - margin) {
      doc.addPage()
      yPosition = margin
      return true
    }
    return false
  }

  function addWrappedText(text, x, y, maxWidth, lineHeight = 6) {
    const lines = doc.splitTextToSize(text, maxWidth)
    doc.text(lines, x, y)
    return lines.length * lineHeight
  }

  function addSectionHeader(text, y) {
    doc.setFontSize(18)
    doc.setFont('helvetica', 'bold')
    doc.setTextColor(5, 150, 105) // emerald color
    doc.text(text, margin, y)
    doc.setDrawColor(5, 150, 105)
    doc.line(margin, y + 2, margin + 50, y + 2)
    doc.setTextColor(0, 0, 0) // reset to black
    return y + 15
  }

  // Cover Page
  doc.setFontSize(28)
  doc.setFont('helvetica', 'bold')
  doc.setTextColor(5, 150, 105) // emerald color
  doc.text('CARBON FOOTPRINT', pageWidth / 2, 80, { align: 'center' })
  doc.text('ANALYSIS REPORT', pageWidth / 2, 95, { align: 'center' })
  doc.setTextColor(0, 0, 0) // reset to black

  doc.setFontSize(14)
  doc.setFont('helvetica', 'normal')
  doc.text('Supply Chain Sustainability Assessment', pageWidth / 2, 115, { align: 'center' })

  // Company info placeholder
  doc.setFontSize(12)
  doc.text('Prepared for: [Company Name]', pageWidth / 2, 140, { align: 'center' })
  doc.text(`Report Date: ${new Date().toLocaleDateString()}`, pageWidth / 2, 150, {
    align: 'center',
  })

  // Footer on cover page
  doc.setFontSize(10)
  doc.text('Confidential - Internal Use Only', pageWidth / 2, pageHeight - 20, { align: 'center' })

  // Table of Contents Page
  doc.addPage()
  yPosition = margin

  yPosition = addSectionHeader('TABLE OF CONTENTS', yPosition)

  doc.setFontSize(12)
  doc.setFont('helvetica', 'normal')

  const tocItems = [
    { title: 'Executive Summary', page: 2 },
    { title: 'Key Findings', page: 2 },
    { title: 'Detailed Analysis', page: 3 },
    { title: 'Recommendations', page: 4 },
    { title: 'Implementation Guidelines', page: 5 },
    { title: 'Conclusion', page: 6 },
  ]

  tocItems.forEach((item) => {
    doc.text(item.title, margin, yPosition)
    // Dotted line
    const dots = '.'.repeat(50)
    doc.text(dots, margin + 70, yPosition)
    doc.text(`${item.page}`, pageWidth - margin - 10, yPosition, { align: 'right' })
    yPosition += 8
  })

  yPosition += 20

  // Executive Summary Page
  doc.addPage()
  yPosition = margin

  yPosition = addSectionHeader('EXECUTIVE SUMMARY', yPosition)

  doc.setFontSize(12)
  doc.setFont('helvetica', 'normal')

  const executiveSummary = `This report presents a comprehensive analysis of your organization's carbon footprint across the supply chain. The assessment identifies key emission sources and provides actionable recommendations for sustainability improvement.`

  yPosition += addWrappedText(executiveSummary, margin, yPosition, contentWidth, 6)
  yPosition += 10

  // Key Metrics Box
  doc.setDrawColor(5, 150, 105)
  doc.setFillColor(229, 250, 240) // light emerald
  doc.roundedRect(margin, yPosition, contentWidth, 40, 3, 3, 'F')
  doc.roundedRect(margin, yPosition, contentWidth, 40, 3, 3, 'S')

  doc.setFontSize(14)
  doc.setFont('helvetica', 'bold')
  doc.text('KEY FINDINGS', margin + 10, yPosition + 10)

  doc.setFontSize(12)
  doc.setFont('helvetica', 'normal')
  doc.text(
    `• Total Carbon Footprint: ${props.total} kg CO₂ equivalent`,
    margin + 10,
    yPosition + 20,
  )

  if (props.industryAverage) {
    doc.text(`• Industry Average: ${props.industryAverage} kg CO₂`, margin + 10, yPosition + 27)
  }

  if (props.improvement) {
    doc.text(`• Improvement Potential: ${props.improvement}`, margin + 10, yPosition + 34)
  }

  yPosition += 50

  // Detailed Analysis
  checkPageBreak(30)
  yPosition = addSectionHeader('DETAILED ANALYSIS', yPosition)

  doc.setFontSize(14)
  doc.setFont('helvetica', 'bold')
  doc.text('1. Emissions Breakdown by Supply Chain Stage', margin, yPosition)
  yPosition += 10

  if (props.breakdown && props.breakdown.length > 0) {
    doc.setFontSize(12)
    doc.setFont('helvetica', 'normal')

    // Table header
    doc.setFillColor(229, 250, 240)
    doc.rect(margin, yPosition, contentWidth, 8, 'F')
    doc.setFont('helvetica', 'bold')
    doc.text('Supply Chain Stage', margin, yPosition + 6)
    doc.text('Emissions (kg CO₂)', margin + 60, yPosition + 6)
    doc.text('Percentage', margin + 130, yPosition + 6)
    yPosition += 8

    // Table separator line
    doc.setDrawColor(5, 150, 105)
    doc.line(margin, yPosition, margin + contentWidth, yPosition)
    yPosition += 5

    doc.setFont('helvetica', 'normal')

    // Alternate row colors
    props.breakdown.forEach((item, index) => {
      checkPageBreak(8)
      if (index % 2 === 0) {
        doc.setFillColor(240, 253, 250)
        doc.rect(margin, yPosition - 2, contentWidth, 8, 'F')
      }
      doc.text(item.label, margin, yPosition + 6)
      doc.text(item.value.toFixed(2), margin + 80, yPosition + 6)
      doc.text(`${item.percent}%`, margin + 130, yPosition + 6)
      yPosition += 8
    })

    yPosition += 10

    // Analysis text
    const largestContributor = props.breakdown.reduce((max, item) =>
      item.value > max.value ? item : max,
    )

    const analysisText = `The analysis reveals that ${largestContributor.label} is the largest contributor to your carbon footprint, accounting for ${largestContributor.percent}% of total emissions (${largestContributor.value.toFixed(2)} kg CO₂). This represents a significant opportunity for targeted reduction efforts.`

    yPosition += addWrappedText(analysisText, margin, yPosition, contentWidth, 6)
    yPosition += 10
  }

  // Recommendations Section
  if (props.suggestions && props.suggestions.length > 0) {
    checkPageBreak(30)
    yPosition = addSectionHeader('RECOMMENDATIONS', yPosition)

    doc.setFontSize(12)
    doc.setFont('helvetica', 'normal')

    const introText = `Based on the analysis of your carbon footprint, the following recommendations are proposed to reduce emissions and improve sustainability performance:`
    yPosition += addWrappedText(introText, margin, yPosition, contentWidth, 6)
    yPosition += 10

    props.suggestions.forEach((suggestion, index) => {
      checkPageBreak(20)
      doc.setFont('helvetica', 'bold')
      doc.setTextColor(5, 150, 105)
      doc.text(`${index + 1}. `, margin, yPosition)

      doc.setFont('helvetica', 'normal')
      doc.setTextColor(0, 0, 0)
      const textHeight = addWrappedText(suggestion, margin + 10, yPosition, contentWidth - 10, 6)
      yPosition += textHeight + 5
    })

    yPosition += 10
  }

  // Implementation Guidelines
  checkPageBreak(40)
  yPosition = addSectionHeader('IMPLEMENTATION GUIDELINES', yPosition)

  doc.setFontSize(12)
  doc.setFont('helvetica', 'normal')

  const implementationText = `To maximize the impact of these recommendations, consider the following implementation approach:

  1. Prioritize high-impact, low-cost initiatives for immediate implementation
  2. Develop a phased approach for larger investments
  3. Establish baseline measurements and regular monitoring
  4. Engage suppliers and partners in sustainability initiatives
  5. Consider certification programs (ISO 14001, Carbon Trust, etc.)

  Regular monitoring and reporting will be essential to track progress and identify additional opportunities for improvement.`

  yPosition += addWrappedText(implementationText, margin, yPosition, contentWidth, 6)
  yPosition += 15

  // Conclusion
  checkPageBreak(30)
  yPosition = addSectionHeader('CONCLUSION', yPosition)

  doc.setFontSize(12)
  doc.setFont('helvetica', 'normal')

  const conclusionText = `This carbon footprint analysis provides a comprehensive foundation for your sustainability strategy. With a total footprint of ${props.total} kg CO₂ equivalent, there are clear opportunities for improvement through targeted interventions in key areas identified in this report.

  The recommended actions, when implemented systematically, can significantly reduce your environmental impact while potentially generating cost savings and operational efficiencies. Regular assessment and continuous improvement will be key to long-term success in sustainability goals.`

  yPosition += addWrappedText(conclusionText, margin, yPosition, contentWidth, 6)

  // Add page numbers
  const pageCount = doc.internal.getNumberOfPages()
  for (let i = 1; i <= pageCount; i++) {
    doc.setPage(i)
    doc.setFontSize(10)
    doc.text(`Page ${i} of ${pageCount}`, pageWidth / 2, pageHeight - 10, { align: 'center' })
  }

  // Save the PDF
  const fileName = `carbon-footprint-report-${new Date().toISOString().split('T')[0]}.pdf`
  doc.save(fileName)
}
</script>

<template>
  <div
    class="w-full flex flex-col gap-16 items-center bg-gradient-to-br from-emerald-50 via-teal-50 to-cyan-50 min-h-screen overflow-hidden"
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

    <!-- PDF Content Start -->
    <div ref="pdfContent" class="w-full flex flex-col gap-16 items-center">
      <!-- Header Section -->
      <section
        class="w-full flex flex-col items-center justify-center text-center py-12 px-4 relative z-10"
      >
        <div class="space-y-6 max-w-4xl" :class="{ 'animate-fade-in-up': isVisible }">
          <h1
            class="text-4xl md:text-6xl font-black text-transparent bg-clip-text bg-gradient-to-r from-emerald-600 via-teal-600 to-cyan-600 leading-tight tracking-tight"
          >
            Carbon Footprint
            <span class="text-emerald-800">Analysis</span>
          </h1>
          <p class="text-lg md:text-xl text-slate-600 max-w-3xl leading-relaxed font-medium">
            Your comprehensive supply chain carbon footprint analysis is complete.
            <span class="text-emerald-600 font-semibold"
              >Review insights and optimization opportunities.</span
            >
          </p>
        </div>
      </section>

      <!-- Total Carbon Footprint -->
      <section class="w-full max-w-4xl flex flex-col items-center gap-8 px-4 relative z-10">
        <div
          class="relative w-full bg-white/80 backdrop-blur-sm rounded-3xl shadow-2xl p-8 border border-white/50 overflow-hidden"
          :class="{ 'animate-fade-in-up': isVisible }"
        >
          <div
            class="absolute inset-0 bg-gradient-to-br from-emerald-400/10 to-teal-400/10 rounded-3xl"
          ></div>
          <div class="relative z-10 text-center">
            <div
              class="inline-flex items-center justify-center w-20 h-20 bg-gradient-to-r from-emerald-500 to-teal-500 rounded-full mb-6"
            >
              <svg
                class="w-10 h-10 text-white"
                fill="none"
                stroke="currentColor"
                viewBox="0 0 24 24"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M13 10V3L4 14h7v7l9-11h-7z"
                ></path>
              </svg>
            </div>
            <h2 class="text-2xl font-bold text-emerald-700 mb-4">Total Carbon Footprint</h2>
            <div
              class="text-6xl md:text-7xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-emerald-600 to-teal-600 mb-2 animate-bounce-in"
            >
              {{ total }}
            </div>
            <div class="text-xl text-slate-600 font-medium">kg CO₂ equivalent</div>
            <div v-if="industryAverage" class="mt-2 text-emerald-700 text-base font-semibold">
              Industry average: <span class="font-bold">{{ industryAverage }} kg CO₂</span>
            </div>
            <div
              v-if="improvement"
              class="mt-2 text-cyan-700 text-base font-semibold animate-fade-in"
            >
              <span class="inline-block bg-cyan-100 rounded px-2 py-1">{{ improvement }}</span>
            </div>
          </div>
        </div>
      </section>

      <!-- After the Total Carbon Footprint section -->
      <section
        class="w-full max-w-4xl flex flex-col items-center gap-8 px-4 relative z-10 break-after"
      >
        <div
          class="relative w-full bg-white/90 backdrop-blur-sm rounded-3xl shadow-xl p-8 border border-white/50"
        >
          <h2 class="text-2xl font-bold text-emerald-700 mb-6 text-center">Key Findings Summary</h2>

          <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            <div class="bg-emerald-50 rounded-xl p-6 text-center">
              <div class="text-3xl font-bold text-emerald-700 mb-2">{{ total }}</div>
              <div class="text-sm font-medium text-emerald-600">Total CO₂ Emissions</div>
            </div>

            <div class="bg-teal-50 rounded-xl p-6 text-center">
              <div class="text-3xl font-bold text-teal-700 mb-2">
                {{ breakdown[0]?.percent || '0' }}%
              </div>
              <div class="text-sm font-medium text-teal-600">Largest Contributor</div>
              <div class="text-xs text-teal-500 mt-1">{{ breakdown[0]?.label || 'N/A' }}</div>
            </div>

            <div v-if="improvement" class="bg-cyan-50 rounded-xl p-6 text-center">
              <div class="text-xl font-bold text-cyan-700 mb-2">Potential Savings</div>
              <div class="text-sm text-cyan-600">{{ improvement }}</div>
            </div>
          </div>

          <div class="mt-8">
            <h3 class="text-lg font-semibold text-slate-800 mb-3">Emissions Breakdown</h3>
            <table class="w-full border-collapse">
              <thead>
                <tr class="bg-emerald-100 text-emerald-800 text-sm">
                  <th class="p-3 text-left rounded-tl-xl">Stage</th>
                  <th class="p-3 text-right">CO₂ (kg)</th>
                  <th class="p-3 text-right rounded-tr-xl">Percentage</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(b, index) in breakdown"
                  :key="b.label"
                  :class="
                    index === breakdown.length - 1 ? 'border-b-0' : 'border-b border-emerald-50'
                  "
                >
                  <td class="p-3 text-sm text-slate-700">{{ b.label }}</td>
                  <td class="p-3 text-sm text-slate-700 text-right">{{ b.value.toFixed(2) }}</td>
                  <td class="p-3 text-sm text-slate-700 text-right">{{ b.percent }}%</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </section>

      <!-- Charts Section -->
      <section
        v-if="breakdown.length && pieData.datasets.length"
        class="w-full max-w-7xl flex flex-col items-center gap-8 px-4 relative z-10"
      >
        <div class="text-center">
          <h2 class="text-3xl md:text-4xl font-bold text-slate-800 mb-4">Emissions Breakdown</h2>
          <p class="text-lg text-slate-600 max-w-2xl">
            Detailed analysis of your carbon footprint across different supply chain stages
          </p>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 w-full">
          <!-- Pie Chart -->
          <div
            class="group relative bg-white/80 backdrop-blur-sm rounded-3xl shadow-xl hover:shadow-2xl transition-all duration-500 transform hover:-translate-y-2 p-8 border border-white/20"
          >
            <div
              class="absolute inset-0 bg-gradient-to-br from-emerald-50 to-teal-50 rounded-3xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
            ></div>
            <div class="relative z-10">
              <h3 class="text-xl font-bold text-slate-800 mb-6 text-center">Emissions by Stage</h3>
              <div class="flex justify-center">
                <Pie
                  :data="pieData"
                  :options="pieOptions"
                  style="max-width: 350px; max-height: 350px"
                />
              </div>
              <ul class="mt-4 space-y-1 w-full">
                <li
                  v-for="b in breakdown"
                  :key="b.label"
                  class="flex justify-between text-emerald-800 text-sm"
                >
                  <span>{{ b.label }}</span>
                  <span>{{ b.value.toFixed(2) }} kg ({{ b.percent }}%)</span>
                </li>
              </ul>
            </div>
          </div>

          <!-- Bar Chart -->
          <div
            class="group relative bg-white/80 backdrop-blur-sm rounded-3xl shadow-xl hover:shadow-2xl transition-all duration-500 transform hover:-translate-y-2 p-8 border border-white/20"
          >
            <div
              class="absolute inset-0 bg-gradient-to-br from-teal-50 to-cyan-50 rounded-3xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
            ></div>
            <div class="relative z-10">
              <h3 class="text-xl font-bold text-slate-800 mb-6 text-center">Stage Comparison</h3>
              <div class="flex justify-center">
                <Bar
                  :data="barData"
                  :options="barOptions"
                  style="max-width: 350px; max-height: 350px"
                />
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- No Data Message -->
      <section v-else class="w-full max-w-4xl flex flex-col items-center gap-8 px-4 relative z-10">
        <div
          class="relative w-full bg-white/80 backdrop-blur-sm rounded-3xl shadow-xl p-12 border border-white/50 text-center"
        >
          <div class="text-6xl mb-4">📊</div>
          <h3 class="text-2xl font-bold text-slate-800 mb-4">No Data Available</h3>
          <p class="text-lg text-slate-600 mb-6">
            Please complete the carbon footprint calculation form to see your detailed analysis.
          </p>
          <button
            class="bg-gradient-to-r from-emerald-500 to-teal-500 text-white px-8 py-3 rounded-2xl font-semibold shadow-lg hover:shadow-emerald-500/25 transform hover:scale-105 transition-all duration-300"
            @click="$emit('start-over')"
          >
            Start Analysis
          </button>
        </div>
      </section>

      <!-- Optimization Suggestions -->
      <section
        v-if="suggestions.length"
        class="w-full max-w-6xl flex flex-col items-center gap-8 px-4 relative z-10"
      >
        <div class="text-center">
          <h2 class="text-3xl md:text-4xl font-bold text-slate-800 mb-4">
            Optimization Recommendations
          </h2>
          <p class="text-lg text-slate-600 max-w-3xl">
            Personalized suggestions to reduce your carbon footprint and improve sustainability
          </p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-6 w-full">
          <div
            v-for="(suggestion, index) in suggestions"
            :key="index"
            class="group relative bg-white/80 backdrop-blur-sm rounded-2xl shadow-lg hover:shadow-xl transition-all duration-300 transform hover:-translate-y-1 p-6 border border-white/50"
          >
            <div
              class="absolute inset-0 bg-gradient-to-br from-emerald-400/10 to-teal-400/10 rounded-2xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
            ></div>
            <div class="relative z-10">
              <div class="flex items-start gap-4">
                <div
                  class="flex-shrink-0 w-8 h-8 bg-emerald-500 rounded-full flex items-center justify-center text-white font-bold text-sm"
                >
                  {{ index + 1 }}
                </div>
                <div>
                  <p class="text-slate-700 leading-relaxed">{{ suggestion }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Action Section -->
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
                Ready for Another Analysis?
              </h2>
              <p class="text-xl text-slate-600 max-w-2xl mx-auto">
                Continue optimizing your supply chain with additional carbon footprint calculations.
              </p>

              <div class="flex flex-col sm:flex-row gap-4 justify-center items-center pt-4">
                <button
                  class="group relative bg-gradient-to-r from-emerald-500 to-teal-500 text-white px-10 py-4 rounded-2xl text-lg font-semibold shadow-xl hover:shadow-emerald-500/25 transform hover:scale-105 transition-all duration-300"
                  @click="$emit('start-over')"
                >
                  <span class="flex items-center gap-2">
                    New Analysis
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
                        d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"
                      ></path>
                    </svg>
                  </span>
                </button>

                <button
                  class="group bg-white/80 backdrop-blur-sm text-emerald-700 px-8 py-4 rounded-2xl text-lg font-semibold border-2 border-emerald-200 hover:border-emerald-300 hover:bg-white transition-all duration-300"
                  @click="exportPDF"
                >
                  <span class="flex items-center gap-2">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M12 10v6m0 0l-3-3m3 3l3-3m2 8H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"
                      ></path>
                    </svg>
                    Export Report
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
                  <span class="text-sm font-medium">Comprehensive analysis</span>
                </div>
                <div class="w-1 h-1 bg-emerald-300 rounded-full"></div>
                <div class="flex items-center gap-2 text-emerald-600">
                  <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                    <path
                      fill-rule="evenodd"
                      d="M3 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z"
                      clip-rule="evenodd"
                    ></path>
                  </svg>
                  <span class="text-sm font-medium">Detailed recommendations</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
    <!-- PDF Content End -->

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

@keyframes bounce-in {
  0% {
    transform: scale(0.7);
    opacity: 0;
  }
  60% {
    transform: scale(1.1);
    opacity: 1;
  }
  100% {
    transform: scale(1);
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

.animate-bounce-in {
  animation: bounce-in 0.7s cubic-bezier(0.4, 2, 0.6, 1) both;
}
</style>
