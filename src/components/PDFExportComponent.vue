<template>
  <div class="w-full">
    <!-- PDF Export Button -->
    <div class="flex justify-center mb-8">
      <button
        @click="exportPDF"
        :disabled="isExporting"
        class="btn btn-primary gap-2 shadow-lg hover:shadow-xl transition-all duration-300"
        :class="{ loading: isExporting }"
      >
        <svg
          v-if="!isExporting"
          class="w-5 h-5"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M12 10v6m0 0l-3-3m3 3l3-3m2 8H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"
          ></path>
        </svg>
        {{ isExporting ? 'Generating PDF...' : 'Export PDF Report' }}
      </button>
    </div>

    <!-- PDF Content (Hidden from view but used for PDF generation) -->
    <div ref="pdfContent" class="pdf-content">
      <div class="pdf-page">
        <!-- Header -->
        <div class="pdf-header">
          <div class="text-center mb-12">
            <h1 class="pdf-title">Carbon Footprint Analysis Report</h1>
            <p class="pdf-subtitle">Supply Chain Environmental Impact Assessment</p>
            <div class="pdf-date">Generated on {{ formatDate(new Date()) }}</div>
          </div>
        </div>

        <!-- Executive Summary -->
        <div class="pdf-section">
          <h2 class="pdf-section-title">Executive Summary</h2>
          <div class="pdf-summary-card">
            <div class="pdf-summary-metric">
              <div class="pdf-metric-value">{{ total }}</div>
              <div class="pdf-metric-label">kg CO₂ equivalent</div>
            </div>
            <div class="pdf-summary-details">
              <div v-if="industryAverage" class="pdf-detail-item">
                <strong>Industry Average:</strong> {{ industryAverage }} kg CO₂
              </div>
              <div v-if="improvement" class="pdf-detail-item">
                <strong>Optimization Opportunity:</strong> {{ improvement }}
              </div>
            </div>
          </div>
        </div>

        <!-- Breakdown Analysis -->
        <div class="pdf-section">
          <h2 class="pdf-section-title">Emissions Breakdown</h2>
          <div class="pdf-breakdown-table">
            <div class="pdf-table-header">
              <div class="pdf-table-cell">Stage</div>
              <div class="pdf-table-cell">Emissions (kg CO₂)</div>
              <div class="pdf-table-cell">Percentage</div>
            </div>
            <div v-for="item in breakdown" :key="item.label" class="pdf-table-row">
              <div class="pdf-table-cell">{{ item.label }}</div>
              <div class="pdf-table-cell">{{ Number(item.value).toFixed(2) }}</div>
              <div class="pdf-table-cell">{{ item.percent }}%</div>
            </div>
          </div>
        </div>

        <!-- Visual Chart Placeholder -->
        <div class="pdf-section">
          <h2 class="pdf-section-title">Visual Analysis</h2>
          <div class="pdf-chart-placeholder">
            <canvas ref="chartCanvas" width="600" height="300"></canvas>
          </div>
        </div>

        <!-- Product Details -->
        <div class="pdf-section">
          <h2 class="pdf-section-title">Product Information</h2>
          <div class="pdf-info-grid">
            <div class="pdf-info-item">
              <strong>Product Type:</strong> {{ productInfo.type || 'N/A' }}
            </div>
            <div class="pdf-info-item">
              <strong>Weight:</strong> {{ productInfo.weight || 'N/A' }} kg
            </div>
            <div class="pdf-info-item">
              <strong>Quantity:</strong> {{ productInfo.quantity || 'N/A' }}
            </div>
            <div class="pdf-info-item">
              <strong>Material:</strong> {{ rawInfo.material || 'N/A' }}
            </div>
            <div class="pdf-info-item">
              <strong>Source Country:</strong> {{ rawInfo.country || 'N/A' }}
            </div>
            <div class="pdf-info-item">
              <strong>Manufacturing Location:</strong> {{ manufacturingInfo.location || 'N/A' }}
            </div>
            <div class="pdf-info-item">
              <strong>Energy Source:</strong> {{ manufacturingInfo.energy || 'N/A' }}
            </div>
            <div class="pdf-info-item">
              <strong>Transport Mode:</strong> {{ transportInfo.mode || 'N/A' }}
            </div>
            <div class="pdf-info-item">
              <strong>Distance:</strong> {{ transportInfo.distance || 'N/A' }} km
            </div>
          </div>
        </div>

        <!-- Recommendations -->
        <div v-if="suggestions.length" class="pdf-section">
          <h2 class="pdf-section-title">Optimization Recommendations</h2>
          <div class="pdf-recommendations">
            <div
              v-for="(suggestion, index) in suggestions"
              :key="index"
              class="pdf-recommendation-item"
            >
              <div class="pdf-recommendation-number">{{ index + 1 }}</div>
              <div class="pdf-recommendation-text">{{ suggestion }}</div>
            </div>
          </div>
        </div>

        <!-- Footer -->
        <div class="pdf-footer">
          <div class="pdf-footer-text">
            This report was generated using advanced carbon footprint calculation methodologies. For
            more information, please contact your sustainability team.
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import html2pdf from 'html2pdf.js'

const props = defineProps({
  total: { type: String, default: '0' },
  breakdown: { type: Array, default: () => [] },
  suggestions: { type: Array, default: () => [] },
  industryAverage: { type: String, default: '' },
  improvement: { type: String, default: '' },
  productInfo: { type: Object, default: () => ({}) },
  rawInfo: { type: Object, default: () => ({}) },
  manufacturingInfo: { type: Object, default: () => ({}) },
  transportInfo: { type: Object, default: () => ({}) },
})

const pdfContent = ref(null)
const chartCanvas = ref(null)
const isExporting = ref(false)

const formatDate = (date) => {
  return new Intl.DateTimeFormat('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    hour: '2-digit',
    minute: '2-digit',
  }).format(date)
}

const drawChart = () => {
  if (!chartCanvas.value || !props.breakdown.length) return

  const canvas = chartCanvas.value
  const ctx = canvas.getContext('2d')
  const centerX = canvas.width / 2
  const centerY = canvas.height / 2
  const radius = Math.min(centerX, centerY) - 40

  // Clear canvas
  ctx.clearRect(0, 0, canvas.width, canvas.height)

  // Calculate total
  const total = props.breakdown.reduce((sum, item) => sum + Number(item.value), 0)

  // Colors for each segment
  const colors = ['#10b981', '#2dd4bf', '#22d3ee', '#60a5fa', '#a78bfa']

  let currentAngle = -Math.PI / 2 // Start from top

  // Draw pie chart
  props.breakdown.forEach((item, index) => {
    const sliceAngle = (Number(item.value) / total) * 2 * Math.PI

    ctx.beginPath()
    ctx.moveTo(centerX, centerY)
    ctx.arc(centerX, centerY, radius, currentAngle, currentAngle + sliceAngle)
    ctx.closePath()
    ctx.fillStyle = colors[index % colors.length]
    ctx.fill()
    ctx.strokeStyle = '#ffffff'
    ctx.lineWidth = 2
    ctx.stroke()

    // Add labels
    const labelAngle = currentAngle + sliceAngle / 2
    const labelX = centerX + Math.cos(labelAngle) * (radius + 20)
    const labelY = centerY + Math.sin(labelAngle) * (radius + 20)

    ctx.fillStyle = '#374151'
    ctx.font = '12px Arial'
    ctx.textAlign = 'center'
    ctx.fillText(item.label, labelX, labelY)
    ctx.fillText(`${item.percent}%`, labelX, labelY + 15)

    currentAngle += sliceAngle
  })

  // Add title
  ctx.fillStyle = '#374151'
  ctx.font = 'bold 16px Arial'
  ctx.textAlign = 'center'
  ctx.fillText('Emissions Breakdown', centerX, 30)
}

const exportPDF = async () => {
  if (!pdfContent.value) return

  isExporting.value = true

  try {
    // Draw the chart before generating PDF
    drawChart()

    // Wait a bit for chart to render
    await new Promise((resolve) => setTimeout(resolve, 100))

    const options = {
      margin: [0.5, 0.5, 0.5, 0.5],
      filename: `carbon-footprint-report-${new Date().toISOString().split('T')[0]}.pdf`,
      image: { type: 'jpeg', quality: 0.98 },
      html2canvas: {
        scale: 2,
        useCORS: true,
        letterRendering: true,
        allowTaint: true,
      },
      jsPDF: {
        unit: 'in',
        format: 'a4',
        orientation: 'portrait',
        compress: true,
      },
      pagebreak: {
        mode: ['avoid-all', 'css', 'legacy'],
        avoid: ['.pdf-section', '.pdf-summary-card'],
      },
    }

    await html2pdf().set(options).from(pdfContent.value).save()
  } catch (error) {
    console.error('PDF generation failed:', error)
    alert('Failed to generate PDF. Please try again.')
  } finally {
    isExporting.value = false
  }
}

onMounted(() => {
  drawChart()
})

watch(() => props.breakdown, drawChart, { deep: true })
</script>

<style scoped>
.pdf-content {
  position: absolute;
  left: -9999px;
  top: -9999px;
  width: 8.5in;
  background: white;
  font-family: Arial, sans-serif;
  color: #374151;
  line-height: 1.6;
}

.pdf-page {
  padding: 0.5in;
  min-height: 11in;
}

.pdf-header {
  border-bottom: 3px solid #10b981;
  padding-bottom: 1rem;
}

.pdf-title {
  font-size: 28px;
  font-weight: bold;
  color: #10b981;
  margin: 0 0 0.5rem 0;
}

.pdf-subtitle {
  font-size: 16px;
  color: #6b7280;
  margin: 0 0 0.5rem 0;
}

.pdf-date {
  font-size: 14px;
  color: #9ca3af;
  margin: 0;
}

.pdf-section {
  margin: 2rem 0;
  page-break-inside: avoid;
}

.pdf-section-title {
  font-size: 20px;
  font-weight: bold;
  color: #374151;
  margin: 0 0 1rem 0;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid #e5e7eb;
}

.pdf-summary-card {
  background: #f9fafb;
  padding: 1.5rem;
  border-radius: 8px;
  border: 1px solid #e5e7eb;
  display: flex;
  align-items: center;
  gap: 2rem;
}

.pdf-summary-metric {
  text-align: center;
  flex-shrink: 0;
}

.pdf-metric-value {
  font-size: 36px;
  font-weight: bold;
  color: #10b981;
  line-height: 1;
}

.pdf-metric-label {
  font-size: 14px;
  color: #6b7280;
  margin-top: 0.5rem;
}

.pdf-summary-details {
  flex: 1;
}

.pdf-detail-item {
  margin: 0.5rem 0;
  font-size: 14px;
}

.pdf-breakdown-table {
  width: 100%;
  border-collapse: collapse;
  margin: 1rem 0;
}

.pdf-table-header {
  display: flex;
  background: #f3f4f6;
  font-weight: bold;
  border-bottom: 2px solid #d1d5db;
}

.pdf-table-row {
  display: flex;
  border-bottom: 1px solid #e5e7eb;
}

.pdf-table-row:nth-child(even) {
  background: #f9fafb;
}

.pdf-table-cell {
  flex: 1;
  padding: 0.75rem;
  text-align: left;
}

.pdf-table-cell:nth-child(2),
.pdf-table-cell:nth-child(3) {
  text-align: center;
}

.pdf-chart-placeholder {
  text-align: center;
  padding: 1rem;
  background: #f9fafb;
  border-radius: 8px;
  border: 1px solid #e5e7eb;
}

.pdf-info-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin: 1rem 0;
}

.pdf-info-item {
  padding: 0.75rem;
  background: #f9fafb;
  border-radius: 6px;
  border: 1px solid #e5e7eb;
  font-size: 14px;
}

.pdf-recommendations {
  space-y: 1rem;
}

.pdf-recommendation-item {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  padding: 1rem;
  background: #f0f9ff;
  border-radius: 8px;
  border: 1px solid #bae6fd;
  margin-bottom: 1rem;
}

.pdf-recommendation-number {
  flex-shrink: 0;
  width: 2rem;
  height: 2rem;
  background: #10b981;
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  font-size: 14px;
}

.pdf-recommendation-text {
  flex: 1;
  font-size: 14px;
  line-height: 1.5;
}

.pdf-footer {
  margin-top: 3rem;
  padding-top: 1rem;
  border-top: 1px solid #e5e7eb;
  page-break-inside: avoid;
}

.pdf-footer-text {
  font-size: 12px;
  color: #6b7280;
  text-align: center;
  font-style: italic;
}

/* Ensure proper page breaks */
.pdf-section:not(:last-child) {
  page-break-after: auto;
}

.pdf-section h2 {
  page-break-after: avoid;
}
</style>
