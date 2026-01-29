<template>
  <div class="space-y-3 h-full">
    <!-- 左侧区域1：养殖指标 -->
    <div class="backdrop-blur-md bg-gray-800/50 rounded-xl p-3 h-[50%]">
      <h2 class="text-base font-semibold mb-3 text-cyan-300">养殖指标</h2>
      <div class="grid grid-cols-3 gap-3 h-[calc(100%-30px)]">
        <!-- pH 卡片 -->
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col items-center justify-center">
          <div class="text-2xl mb-2 text-cyan-400">💧</div>
          <div :class="getColorClass('ph', phValue)" class="text-2xl font-bold mb-1">
            {{ phValue.toFixed(1) }}
          </div>
          <div class="text-xs text-gray-400">pH 值</div>
        </div>

        <!-- ORP 卡片 -->
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col items-center justify-center">
          <div class="text-2xl mb-2 text-cyan-400">⚡</div>
          <div :class="getColorClass('orp', orpValue)" class="text-2xl font-bold mb-1">
            {{ Math.round(orpValue) }}
          </div>
          <div class="text-xs text-gray-400">ORP (mV)</div>
        </div>

        <!-- 水温 卡片 -->
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col items-center justify-center">
          <div class="text-2xl mb-2 text-cyan-400">🌡️</div>
          <div :class="getColorClass('temp', tempValue)" class="text-2xl font-bold mb-1">
            {{ tempValue.toFixed(1) }}
          </div>
          <div class="text-xs text-gray-400">水温 (°C)</div>
        </div>

        <!-- 溶氧 卡片 -->
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col items-center justify-center">
          <div class="text-2xl mb-2 text-cyan-400">💨</div>
          <div :class="getColorClass('do', doValue)" class="text-2xl font-bold mb-1">
            {{ doValue.toFixed(1) }}
          </div>
          <div class="text-xs text-gray-400">溶氧 (mg/L)</div>
        </div>

        <!-- 气温 卡片 -->
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col items-center justify-center">
          <div class="text-2xl mb-2 text-cyan-400">☀️</div>
          <div :class="getColorClass('airTemp', airTempValue)" class="text-2xl font-bold mb-1">
            {{ airTempValue.toFixed(1) }}
          </div>
          <div class="text-xs text-gray-400">气温 (°C)</div>
        </div>

        <!-- 氨氮 卡片 -->
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col items-center justify-center">
          <div class="text-2xl mb-2 text-cyan-400">☢️</div>
          <div :class="getColorClass('ammonia', ammoniaValue)" class="text-2xl font-bold mb-1">
            {{ ammoniaValue.toFixed(2) }}
          </div>
          <div class="text-xs text-gray-400">氨氮 (mg/L)</div>
        </div>
      </div>
    </div>

    <!-- 左侧区域2：数据趋势 -->
    <div class="backdrop-blur-md bg-gray-800/50 rounded-xl p-3 h-[50%]">
      <h2 class="text-base font-semibold mb-3 text-cyan-300">数据趋势</h2>
      <div class="h-[calc(100%-30px)]">
        <!-- 主要趋势图表 -->
        <div class="h-full" ref="mainChartRef"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import * as echarts from 'echarts'
import { ref, onMounted, onUnmounted, computed } from 'vue'

// 指标数值
const phValue = ref(7.2)
const orpValue = ref(320)
const tempValue = ref(25.5)
const doValue = ref(6.8)
const airTempValue = ref(28.5)
const ammoniaValue = ref(0.2)

// 图表引用
const mainChartRef = ref(null)
let mainChart = null

// 安全范围配置
const safeRanges = {
  ph: { min: 6.5, max: 8.5 },
  orp: { min: 200, max: 400 },
  temp: { min: 20, max: 30 },
  do: { min: 5, max: 10 },
  airTemp: { min: 20, max: 35 },
  ammonia: { min: 0, max: 0.5 }
}

// 根据数值获取颜色类
const getColorClass = (type, value) => {
  const range = safeRanges[type]
  if (value >= range.min && value <= range.max) {
    return 'text-cyan-400'
  } else {
    return 'text-orange-500'
  }
}

// 初始化主图表
const initMainChart = () => {
  if (mainChartRef.value) {
    mainChart = echarts.init(mainChartRef.value)
    
    const option = {
      grid: {
        left: '5%',
        right: '5%',
        top: '10%',
        bottom: '15%'
      },
      xAxis: {
        type: 'category',
        data: ['00:00', '04:00', '08:00', '12:00', '16:00', '20:00'],
        axisLine: {
          lineStyle: {
            color: '#4b5563'
          }
        },
        axisLabel: {
          color: '#9ca3af'
        }
      },
      yAxis: {
        type: 'value',
        axisLine: {
          lineStyle: {
            color: '#4b5563'
          }
        },
        axisLabel: {
          color: '#9ca3af'
        },
        splitLine: {
          lineStyle: {
            color: '#374151'
          }
        }
      },
      tooltip: {
        trigger: 'axis'
      },
      series: [
        {
          name: '水温',
          type: 'line',
          data: [24.5, 24.2, 25.0, 25.8, 26.0, 25.5],
          smooth: true,
          lineStyle: {
            color: '#22d3ee'
          },
          areaStyle: {
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              { offset: 0, color: 'rgba(34, 211, 238, 0.3)' },
              { offset: 1, color: 'rgba(34, 211, 238, 0.05)' }
            ])
          }
        },
        {
          name: '溶氧',
          type: 'line',
          data: [7.0, 7.2, 6.5, 6.2, 6.8, 7.0],
          smooth: true,
          lineStyle: {
            color: '#4ade80'
          },
          areaStyle: {
            color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
              { offset: 0, color: 'rgba(74, 222, 128, 0.3)' },
              { offset: 1, color: 'rgba(74, 222, 128, 0.05)' }
            ])
          }
        }
      ]
    }
    
    mainChart.setOption(option)
  }
}

// 响应式调整
const handleResize = () => {
  mainChart?.resize()
}

// 生命周期
onMounted(() => {
  initMainChart()
  window.addEventListener('resize', handleResize)
  
  // 模拟数据变化
  setInterval(() => {
    phValue.value = 7.2 + (Math.random() - 0.5) * 0.5
    orpValue.value = 320 + (Math.random() - 0.5) * 50
    tempValue.value = 25.5 + (Math.random() - 0.5) * 1
    doValue.value = 6.8 + (Math.random() - 0.5) * 0.8
    airTempValue.value = 28.5 + (Math.random() - 0.5) * 2
    ammoniaValue.value = 0.2 + (Math.random() - 0.5) * 0.1
  }, 3000)
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
  mainChart?.dispose()
})
</script>

<style scoped>
/* 自定义样式 */
</style>