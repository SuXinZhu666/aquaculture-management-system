<template>
  <div class="backdrop-blur-md bg-gray-800/50 rounded-xl p-0 h-full">
    <div 
      class="relative h-full rounded-xl border-2 border-dashed border-cyan-500/50 cursor-pointer overflow-hidden"
      @click="triggerFileInput"
    >
      <input 
        ref="fileInputRef"
        type="file" 
        accept="image/*" 
        class="hidden"
        @change="handleImageUpload"
      >
      <!-- 初始占位图 -->
      <div v-if="!uploadedImage" class="absolute inset-0 flex flex-col items-center justify-center text-cyan-400/70">
        <div class="text-5xl mb-3">📷</div>
        <div class="text-base">点击上传养殖场照片</div>
      </div>
      <!-- 上传后的图片 -->
      <img 
        v-else 
        :src="uploadedImage" 
        class="w-full h-full object-cover transition-transform duration-300 hover:scale-105"
        alt="养殖场照片"
      >
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 图片上传相关
const fileInputRef = ref(null)
const uploadedImage = ref(null)

// 触发文件输入
const triggerFileInput = () => {
  fileInputRef.value?.click()
}

// 处理图片上传
const handleImageUpload = (event) => {
  const file = event.target.files[0]
  if (file) {
    const reader = new FileReader()
    reader.onload = (e) => {
      uploadedImage.value = e.target.result
    }
    reader.readAsDataURL(file)
  }
}
</script>

<style scoped>
/* 自定义样式 */
</style>