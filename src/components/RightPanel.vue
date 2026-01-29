<template>
  <div class="space-y-3 h-full">
    <!-- 右侧区域1：综合折线图面板 -->
    <div class="backdrop-blur-md bg-gray-800/50 rounded-xl p-3 h-[50%]">
      <h2 class="text-base font-semibold mb-3 text-cyan-300">综合监测</h2>
      
      <!-- 切换按钮 -->
      <div class="flex space-x-2 mb-3">
        <button 
          v-for="tab in tabs" 
          :key="tab.key"
          class="px-3 py-1 text-xs rounded-full transition-all duration-300"
          :class="activeTab === tab.key ? 'bg-cyan-400 text-gray-900 font-medium' : 'bg-gray-700 text-gray-400'"
          @click="switchTab(tab.key)"
        >
          {{ tab.name }}
        </button>
      </div>
      
      <!-- 折线图容器 -->
      <div class="h-[calc(100%-60px)]">
        <div ref="mainChartRef" class="w-full h-full"></div>
      </div>
    </div>

    <!-- 右侧区域2：告警信息 -->
    <div class="backdrop-blur-md bg-gray-800/50 rounded-xl p-3 h-[50%]">
      <h2 class="text-base font-semibold mb-3 text-cyan-300">告警信息</h2>
      <div 
        ref="alertsContainerRef"
        class="h-[calc(100%-30px)] overflow-hidden"
      >
        <div 
          v-for="alert in alerts" 
          :key="alert.id"
          class="flex items-center justify-between p-2 mb-2 rounded-lg transition-all duration-500 ease-in-out"
          :class="{
            'bg-yellow-500/20 border-l-4 border-yellow-500': alert.level === 'warning',
            'bg-red-500/20 border-l-4 border-red-500': alert.level === 'error',
            'bg-orange-500/20 border-l-4 border-orange-500': alert.level === 'fault'
          }"
        >
          <div class="flex items-center">
            <div 
              class="w-2 h-2 rounded-full mr-3 animate-pulse"
              :class="{
                'bg-yellow-500': alert.level === 'warning',
                'bg-red-500': alert.level === 'error',
                'bg-orange-500': alert.level === 'fault'
              }"
            ></div>
            <div>
              <div class="text-sm font-medium">{{ alert.message }}</div>
              <div class="text-xs text-gray-400">{{ alert.time }}</div>
            </div>
          </div>
          <button 
            @click="handleAlertComplete(alert.id)"
            class="px-3 py-1 text-xs rounded-full bg-gray-700 hover:bg-gray-600 transition-colors duration-300"
          >
            处理完成
          </button>
        </div>
        <div v-if="alerts.length === 0" class="text-center py-6 text-gray-400 text-sm">
          暂无告警信息
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import * as echarts from 'echarts'
import { ref, onMounted, onUnmounted, nextTick, computed } from 'vue'

// 图表引用
const mainChartRef = ref(null)
let mainChart = null

// 告警信息相关
const alertsContainerRef = ref(null)
const alerts = ref([
  { id: 1, level: 'warning', message: 'pH值异常', time: '10:23' },
  { id: 2, level: 'error', message: '溶氧量偏低', time: '10:15' },
  { id: 3, level: 'fault', message: '过滤器故障', time: '09:45' },
  { id: 4, level: 'warning', message: '水温波动较大', time: '09:30' },
  { id: 5, level: 'error', message: '氨氮含量偏高', time: '08:50' }
])

// 切换标签
const tabs = ref([
  { key: 'oxygen-ammonia', name: '溶氧-氨氮' },
  { key: 'temperature', name: '温度组' },
  { key: 'chemical', name: '化学组' }
])
const activeTab = ref('oxygen-ammonia')

// 时间数据
const timeData = ['00:00', '04:00', '08:00', '12:00', '16:00', '20:00', '24:00']

// 生成随机数据
const generateRandomData = (base, range) => {
  return Array.from({ length: timeData.length }, (_, i) => {
    return base + (Math.random() - 0.5) * range * 2
  })
}

// 数据配置
const chartData = {
  'oxygen-ammonia': {
    yAxis: [
      {
        type: 'value',
        name: '溶氧',
        nameTextStyle: {
          color: '#22d3ee'
        },
        axisLabel: {
          color: '#9ca3af'
        },
        axisLine: {
          show: false
        },
        splitLine: {
          show: false
        }
      },
      {
        type: 'value',
        name: '氨氮',
        nameTextStyle: {
          color: '#f97316'
        },
        axisLabel: {
          color: '#9ca3af'
        },
        axisLine: {
          show: false
        },
        splitLine: {
          show: false
        }
      }
    ],
    series: [
      {
        name: '溶氧',
        type: 'line',
        data: generateRandomData(6.8, 1),
        smooth: true,
        symbol: 'circle',
        symbolSize: 6,
        lineStyle: {
          width: 3,
          color: '#22d3ee'
        },
        itemStyle: {
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
        name: '氨氮',
        type: 'line',
        yAxisIndex: 1,
        data: generateRandomData(0.2, 0.1),
        smooth: true,
        symbol: 'circle',
        symbolSize: 6,
        lineStyle: {
          width: 3,
          color: '#f97316'
        },
        itemStyle: {
          color: '#f97316'
        },
        areaStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            { offset: 0, color: 'rgba(249, 115, 22, 0.3)' },
            { offset: 1, color: 'rgba(249, 115, 22, 0.05)' }
          ])
        }
      }
    ]
  },
  'temperature': {
    yAxis: [
      {
        type: 'value',
        name: '水温',
        nameTextStyle: {
          color: '#22d3ee'
        },
        axisLabel: {
          color: '#9ca3af'
        },
        axisLine: {
          show: false
        },
        splitLine: {
          show: false
        }
      },
      {
        type: 'value',
        name: '气温',
        nameTextStyle: {
          color: '#4ade80'
        },
        axisLabel: {
          color: '#9ca3af'
        },
        axisLine: {
          show: false
        },
        splitLine: {
          show: false
        }
      }
    ],
    series: [
      {
        name: '水温',
        type: 'line',
        data: generateRandomData(25.5, 1),
        smooth: true,
        symbol: 'circle',
        symbolSize: 6,
        lineStyle: {
          width: 3,
          color: '#22d3ee'
        },
        itemStyle: {
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
        name: '气温',
        type: 'line',
        yAxisIndex: 1,
        data: generateRandomData(28.5, 2),
        smooth: true,
        symbol: 'circle',
        symbolSize: 6,
        lineStyle: {
          width: 3,
          color: '#4ade80'
        },
        itemStyle: {
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
  },
  'chemical': {
    yAxis: [
      {
        type: 'value',
        name: 'pH',
        nameTextStyle: {
          color: '#22d3ee'
        },
        axisLabel: {
          color: '#9ca3af'
        },
        axisLine: {
          show: false
        },
        splitLine: {
          show: false
        }
      },
      {
        type: 'value',
        name: 'ORP',
        nameTextStyle: {
          color: '#8b5cf6'
        },
        axisLabel: {
          color: '#9ca3af'
        },
        axisLine: {
          show: false
        },
        splitLine: {
          show: false
        }
      }
    ],
    series: [
      {
        name: 'pH',
        type: 'line',
        data: generateRandomData(7.2, 0.5),
        smooth: true,
        symbol: 'circle',
        symbolSize: 6,
        lineStyle: {
          width: 3,
          color: '#22d3ee'
        },
        itemStyle: {
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
        name: 'ORP',
        type: 'line',
        yAxisIndex: 1,
        data: generateRandomData(320, 50),
        smooth: true,
        symbol: 'circle',
        symbolSize: 6,
        lineStyle: {
          width: 3,
          color: '#8b5cf6'
        },
        itemStyle: {
          color: '#8b5cf6'
        },
        areaStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            { offset: 0, color: 'rgba(139, 92, 246, 0.3)' },
            { offset: 1, color: 'rgba(139, 92, 246, 0.05)' }
          ])
        }
      }
    ]
  }
}

// 初始化图表
const initChart = () => {
  if (mainChartRef.value) {
    mainChart = echarts.init(mainChartRef.value)
    updateChart()
  }
}

// 更新图表
const updateChart = () => {
  if (mainChart) {
    const option = {
      animation: true,
      animationDuration: 1000,
      animationEasing: 'cubicOut',
      grid: {
        left: '5%',
        right: '5%',
        top: '10%',
        bottom: '15%'
      },
      xAxis: {
        type: 'category',
        data: timeData,
        axisLabel: {
          color: '#9ca3af'
        },
        axisLine: {
          show: false
        },
        axisTick: {
          show: false
        }
      },
      yAxis: chartData[activeTab.value].yAxis,
      tooltip: {
        trigger: 'axis',
        backgroundColor: 'rgba(30, 41, 59, 0.8)',
        borderColor: 'rgba(34, 211, 238, 0.3)',
        textStyle: {
          color: '#f1f5f9'
        }
      },
      legend: {
        data: chartData[activeTab.value].series.map(item => item.name),
        top: '0%',
        textStyle: {
          color: '#9ca3af'
        }
      },
      series: chartData[activeTab.value].series
    }
    
    mainChart.setOption(option)
  }
}

// 切换标签
const switchTab = (tabKey) => {
  activeTab.value = tabKey
  updateChart()
}

// 处理告警完成
const handleAlertComplete = (id) => {
  const alertIndex = alerts.value.findIndex(alert => alert.id === id)
  if (alertIndex !== -1) {
    // 淡出动画
    const alertElement = alertsContainerRef.value.children[alertIndex]
    if (alertElement) {
      alertElement.style.opacity = '0'
      alertElement.style.transform = 'translateX(-20px)'
      alertElement.style.height = '0'
      alertElement.style.marginBottom = '0'
      alertElement.style.padding = '0'
      
      setTimeout(() => {
        alerts.value.splice(alertIndex, 1)
        nextTick(() => {
          scrollToBottom()
        })
      }, 500)
    }
  }
}

// 滚动到底部
const scrollToBottom = () => {
  if (alertsContainerRef.value) {
    alertsContainerRef.value.scrollTop = alertsContainerRef.value.scrollHeight
  }
}

// 自动滚动
let scrollInterval = null
const startAutoScroll = () => {
  scrollInterval = setInterval(() => {
    if (alertsContainerRef.value) {
      const container = alertsContainerRef.value
      const isAtBottom = container.scrollTop + container.clientHeight >= container.scrollHeight - 10
      
      if (isAtBottom) {
        container.scrollTop = 0
      } else {
        container.scrollTop += 1
      }
    }
  }, 50)
}

// 响应式调整
const handleResize = () => {
  mainChart?.resize()
}

// 生命周期
onMounted(() => {
  initChart()
  window.addEventListener('resize', handleResize)
  nextTick(() => {
    startAutoScroll()
  })
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
  mainChart?.dispose()
  if (scrollInterval) {
    clearInterval(scrollInterval)
  }
})
</script>

<style scoped>
/* 自定义样式 */
</style>