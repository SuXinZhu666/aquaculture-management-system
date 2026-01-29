<template>
  <div class="backdrop-blur-md bg-gray-800/50 rounded-xl p-3 h-full">
    <h2 class="text-base font-semibold mb-3 text-cyan-300">设备管理</h2>
    
    <!-- 设备卡片容器 -->
    <div class="h-[calc(100%-70px)] relative overflow-hidden">
      <!-- 第一页设备 -->
      <div 
        v-if="currentPage === 0"
        class="absolute inset-0 grid grid-cols-5 gap-3 transition-opacity duration-300 ease-in-out"
      >
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col">
          <h3 class="text-sm font-medium mb-3 text-gray-300">增氧机</h3>
          <div class="flex flex-col space-y-2">
            <div class="flex justify-between items-center">
              <span :class="oxygenPumpPower ? 'text-cyan-400' : 'text-gray-400'">主电源</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="oxygenPumpPower">
                <span class="toggle-slider" :class="oxygenPumpPower ? 'active' : ''"></span>
              </label>
            </div>
          </div>
        </div>
        
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col">
          <h3 class="text-sm font-medium mb-3 text-gray-300">水处理系统</h3>
          <div class="flex flex-col space-y-2">
            <div class="flex justify-between items-center">
              <span :class="waterSystemCirculation ? 'text-cyan-400' : 'text-gray-400'">循环</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="waterSystemCirculation">
                <span class="toggle-slider" :class="waterSystemCirculation ? 'active' : ''"></span>
              </label>
            </div>
            <div class="flex justify-between items-center">
              <span :class="waterSystemFilter ? 'text-cyan-400' : 'text-gray-400'">过滤</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="waterSystemFilter">
                <span class="toggle-slider" :class="waterSystemFilter ? 'active' : ''"></span>
              </label>
            </div>
            <div class="flex justify-between items-center">
              <span :class="waterSystemBackwash ? 'text-cyan-400' : 'text-gray-400'">反冲洗</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="waterSystemBackwash">
                <span class="toggle-slider" :class="waterSystemBackwash ? 'active' : ''"></span>
              </label>
            </div>
          </div>
        </div>
        
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col">
          <h3 class="text-sm font-medium mb-3 text-gray-300">投料机</h3>
          <div class="flex flex-col space-y-2">
            <div class="flex justify-between items-center">
              <span :class="feedingMachineTimer ? 'text-cyan-400' : 'text-gray-400'">定时</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="feedingMachineTimer">
                <span class="toggle-slider" :class="feedingMachineTimer ? 'active' : ''"></span>
              </label>
            </div>
            <div class="flex justify-between items-center">
              <span :class="feedingMachineManual ? 'text-cyan-400' : 'text-gray-400'">手动</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="feedingMachineManual">
                <span class="toggle-slider" :class="feedingMachineManual ? 'active' : ''"></span>
              </label>
            </div>
          </div>
        </div>
        
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col">
          <h3 class="text-sm font-medium mb-3 text-gray-300">水泵</h3>
          <div class="flex flex-col space-y-2">
            <div class="flex justify-between items-center">
              <span :class="waterPumpPower ? 'text-cyan-400' : 'text-gray-400'">主电源</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="waterPumpPower">
                <span class="toggle-slider" :class="waterPumpPower ? 'active' : ''"></span>
              </label>
            </div>
          </div>
        </div>
        
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col">
          <h3 class="text-sm font-medium mb-3 text-gray-300">加热器</h3>
          <div class="flex flex-col space-y-2">
            <div class="flex justify-between items-center">
              <span :class="heaterPower ? 'text-cyan-400' : 'text-gray-400'">主电源</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="heaterPower">
                <span class="toggle-slider" :class="heaterPower ? 'active' : ''"></span>
              </label>
            </div>
          </div>
        </div>
      </div>
      
      <!-- 第二页设备 -->
      <div 
        v-else-if="currentPage === 1"
        class="absolute inset-0 grid grid-cols-5 gap-3 transition-opacity duration-300 ease-in-out"
      >
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col">
          <h3 class="text-sm font-medium mb-3 text-gray-300">光照系统</h3>
          <div class="flex flex-col space-y-2">
            <div class="flex justify-between items-center">
              <span :class="lightSystemPower ? 'text-cyan-400' : 'text-gray-400'">主电源</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="lightSystemPower">
                <span class="toggle-slider" :class="lightSystemPower ? 'active' : ''"></span>
              </label>
            </div>
            <div class="flex justify-between items-center">
              <span :class="lightSystemTimer ? 'text-cyan-400' : 'text-gray-400'">定时</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="lightSystemTimer">
                <span class="toggle-slider" :class="lightSystemTimer ? 'active' : ''"></span>
              </label>
            </div>
          </div>
        </div>
        
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col">
          <h3 class="text-sm font-medium mb-3 text-gray-300">气泵</h3>
          <div class="flex flex-col space-y-2">
            <div class="flex justify-between items-center">
              <span :class="airPumpPower ? 'text-cyan-400' : 'text-gray-400'">主电源</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="airPumpPower">
                <span class="toggle-slider" :class="airPumpPower ? 'active' : ''"></span>
              </label>
            </div>
          </div>
        </div>
        
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col">
          <h3 class="text-sm font-medium mb-3 text-gray-300">水质监测</h3>
          <div class="flex flex-col space-y-2">
            <div class="flex justify-between items-center">
              <span :class="waterMonitorPower ? 'text-cyan-400' : 'text-gray-400'">主电源</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="waterMonitorPower">
                <span class="toggle-slider" :class="waterMonitorPower ? 'active' : ''"></span>
              </label>
            </div>
            <div class="flex justify-between items-center">
              <span :class="waterMonitorAlert ? 'text-cyan-400' : 'text-gray-400'">告警</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="waterMonitorAlert">
                <span class="toggle-slider" :class="waterMonitorAlert ? 'active' : ''"></span>
              </label>
            </div>
          </div>
        </div>
        
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col">
          <h3 class="text-sm font-medium mb-3 text-gray-300">消毒系统</h3>
          <div class="flex flex-col space-y-2">
            <div class="flex justify-between items-center">
              <span :class="disinfectionSystemPower ? 'text-cyan-400' : 'text-gray-400'">主电源</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="disinfectionSystemPower">
                <span class="toggle-slider" :class="disinfectionSystemPower ? 'active' : ''"></span>
              </label>
            </div>
            <div class="flex justify-between items-center">
              <span :class="disinfectionSystemAuto ? 'text-cyan-400' : 'text-gray-400'">自动</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="disinfectionSystemAuto">
                <span class="toggle-slider" :class="disinfectionSystemAuto ? 'active' : ''"></span>
              </label>
            </div>
          </div>
        </div>
        
        <div class="bg-gray-700/50 rounded-lg p-3 flex flex-col">
          <h3 class="text-sm font-medium mb-3 text-gray-300">通风系统</h3>
          <div class="flex flex-col space-y-2">
            <div class="flex justify-between items-center">
              <span :class="ventilationSystemPower ? 'text-cyan-400' : 'text-gray-400'">主电源</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="ventilationSystemPower">
                <span class="toggle-slider" :class="ventilationSystemPower ? 'active' : ''"></span>
              </label>
            </div>
            <div class="flex justify-between items-center">
              <span :class="ventilationSystemAuto ? 'text-cyan-400' : 'text-gray-400'">自动</span>
              <label class="toggle-switch">
                <input type="checkbox" v-model="ventilationSystemAuto">
                <span class="toggle-slider" :class="ventilationSystemAuto ? 'active' : ''"></span>
              </label>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 分页控制 -->
    <div class="mt-3 flex justify-center">
      <div class="flex space-x-2">
        <button 
          v-for="(page, index) in totalPages" 
          :key="index"
          class="w-2 h-2 rounded-full transition-all duration-300"
          :class="currentPage === index ? 'bg-cyan-400 w-6' : 'bg-gray-500'"
          @click="currentPage = index"
        ></button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 分页控制
const currentPage = ref(0)
const totalPages = ref(2)

// 设备状态
const oxygenPumpPower = ref(false)

const waterSystemCirculation = ref(false)
const waterSystemFilter = ref(false)
const waterSystemBackwash = ref(false)

const feedingMachineTimer = ref(false)
const feedingMachineManual = ref(false)

const waterPumpPower = ref(false)
const heaterPower = ref(false)

const lightSystemPower = ref(false)
const lightSystemTimer = ref(false)

const airPumpPower = ref(false)

const waterMonitorPower = ref(true)
const waterMonitorAlert = ref(true)

const disinfectionSystemPower = ref(false)
const disinfectionSystemAuto = ref(false)

const ventilationSystemPower = ref(false)
const ventilationSystemAuto = ref(false)
</script>

<style scoped>
/* 切换开关样式 */
.toggle-switch {
  position: relative;
  display: inline-block;
  width: 36px;
  height: 18px;
}

.toggle-switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.toggle-slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #4b5563;
  transition: all 0.3s ease;
  border-radius: 18px;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.3);
}

.toggle-slider:before {
  position: absolute;
  content: "";
  height: 14px;
  width: 14px;
  left: 2px;
  bottom: 2px;
  background-color: #9ca3af;
  transition: all 0.3s ease;
  border-radius: 50%;
}

.toggle-slider.active {
  background-color: rgba(34, 211, 238, 0.3);
  box-shadow: inset 0 2px 4px rgba(34, 211, 238, 0.3), 0 0 8px rgba(34, 211, 238, 0.5);
}

.toggle-slider.active:before {
  transform: translateX(18px);
  background-color: #22d3ee;
  box-shadow: 0 0 8px rgba(34, 211, 238, 0.8);
}

/* 点击动画效果 */
.toggle-switch input:checked + .toggle-slider {
  animation: pulse 0.3s ease;
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
}
</style>