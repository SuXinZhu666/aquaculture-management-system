<template>
  <div class="h-screen w-screen bg-gray-900 text-gray-100 overflow-hidden">
    <!-- 顶部标题栏 -->
    <Header />

    <!-- 主内容区 -->
    <main class="h-[calc(100vh-80px)] w-full p-4 overflow-hidden">
      <div class="h-full grid grid-cols-1 lg:grid-cols-4 gap-4">
        <!-- 左侧：25% -->
        <div class="lg:col-span-1">
          <LeftPanel />
        </div>
        
        <!-- 中间：50% -->
        <div class="lg:col-span-2 space-y-4 h-full">
          <!-- 上部：65% -->
          <div class="h-[65%]">
            <MiddleScene />
          </div>
          <!-- 下部：35% -->
          <div class="h-[35%]">
            <MiddleControl />
          </div>
        </div>
        
        <!-- 右侧：25% -->
        <div class="lg:col-span-1">
          <RightPanel />
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import Header from './components/Header.vue'
import LeftPanel from './components/LeftPanel.vue'
import MiddleScene from './components/MiddleScene.vue'
import MiddleControl from './components/MiddleControl.vue'
import RightPanel from './components/RightPanel.vue'
</script>

<style scoped>
/* 自定义样式 */
</style>