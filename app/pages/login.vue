<script setup>
// 1. Ẩn Header/Footer mặc định
definePageMeta({
  layout: 'empty'
})

// --- STATE ---
const formData = ref({
  email: '',
  password: '',
  remember: false
})

// State lưu lỗi của từng trường
const errors = ref({
  email: '',
  password: ''
})

const isLoading = ref(false)

// --- METHODS ---
const validateForm = () => {
  let isValid = true
  
  // Reset lỗi cũ
  errors.value.email = ''
  errors.value.password = ''

  // Validate Email
  if (!formData.value.email) {
    errors.value.email = 'Vui lòng nhập email hoặc số điện thoại'
    isValid = false
  }

  // Validate Password
  if (!formData.value.password) {
    errors.value.password = 'Vui lòng nhập mật khẩu'
    isValid = false
  } else if (formData.value.password.length < 6) {
    errors.value.password = 'Mật khẩu phải có ít nhất 6 ký tự'
    isValid = false
  }

  return isValid
}

const handleLogin = () => {
  // Gọi hàm validate trước
  if (!validateForm()) return

  isLoading.value = true

  // Giả lập gọi API
  setTimeout(async () => {
    if (formData.value.email === 'admin' && formData.value.password === '123456') {
       await navigateTo('/admin/products')
    } else if (formData.value.email && formData.value.password) {
       // Demo thành công
       await navigateTo('/')
    } else {
       // Lỗi từ server (nếu có) giả lập gán vào password hoặc hiện alert
       errors.value.password = 'Tài khoản hoặc mật khẩu không chính xác'
       isLoading.value = false
    }
  }, 1500)
}
</script>

<template>
  <!-- Dùng h-screen và overflow-hidden để cố định chiều cao -->
  <div class="h-screen flex font-inter text-black bg-white overflow-hidden">
    
    <!-- === CỘT TRÁI: BRAND INTRO (Background Đen) === -->
    <div class="hidden lg:flex w-5/12 bg-black relative items-center justify-center">
      
      <!-- Background Pattern -->
      <div class="absolute inset-0 opacity-30 bg-[url('https://www.transparenttextures.com/patterns/cubes.png')]"></div>
      
      <!-- Brand Content -->
      <div class="z-10 flex flex-col items-center text-center text-white p-8">
        
        <!-- Logo Shield Icon -->
        <div class="mb-6 animate-fade-in-up">
           <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-24 h-24 text-white">
             <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z" />
             <path d="M12 2L20 5V12C20 18 12 22 12 22V2Z" fill="currentColor" stroke="none"/>
           </svg>
        </div>

        <h2 class="text-4xl font-black tracking-tighter mb-4 animate-fade-in-up" style="animation-delay: 0.1s;">
          BW<span class="font-light text-gray-400">STORE</span>
        </h2>
        
        <div class="w-12 h-1 bg-white mb-6 animate-fade-in-up" style="animation-delay: 0.2s;"></div>

        <p class="text-sm text-gray-300 font-light tracking-widest uppercase max-w-xs leading-relaxed animate-fade-in-up" style="animation-delay: 0.3s;">
          Phong cách tối giản.<br>Đẳng cấp khác biệt.
        </p>

        <p class="mt-8 text-[10px] text-gray-500 uppercase tracking-widest animate-fade-in" style="animation-delay: 0.5s;">
          © 2025 BW Store.
        </p>
      </div>
    </div>

    <!-- === CỘT PHẢI: FORM ĐĂNG NHẬP === -->
    <div class="w-full lg:w-7/12 flex flex-col justify-center p-8 lg:p-12 relative bg-white h-full overflow-y-auto">
      
      <!-- Nút Close -->
      <NuxtLink to="/" class="absolute top-6 left-6 text-gray-400 hover:text-black transition">
        <span class="flex items-center gap-2 text-xs font-bold uppercase tracking-wider">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4">
            <path stroke-linecap="round" stroke-linejoin="round" d="M10.5 19.5L3 12m0 0l7.5-7.5M3 12h18" />
          </svg>
          Trang chủ
        </span>
      </NuxtLink>

      <div class="max-w-sm w-full mx-auto">
        
        <!-- Tiêu đề -->
        <div class="mb-6">
          <h1 class="text-3xl font-black uppercase tracking-tighter mb-1 italic">ĐĂNG NHẬP</h1>
          <p class="text-xs text-gray-500 font-medium">Đăng nhập để không bỏ lỡ các ưu đãi độc quyền.</p>
        </div>

        <!-- Form -->
        <form class="space-y-4" @submit.prevent="handleLogin">
          
          <!-- Input Email -->
          <div>
            <div class="relative">
              <input 
                v-model="formData.email"
                type="text" 
                :class="[
                  'w-full h-10 px-3 border text-xs font-bold focus:outline-none focus:ring-1 transition-all rounded-none pr-10',
                  errors.email 
                    ? 'border-red-500 text-red-900 placeholder-red-300 focus:border-red-500 focus:ring-red-500' 
                    : 'border-black bg-white text-black placeholder-gray-400 focus:ring-black'
                ]"
                placeholder="EMAIL HOẶC SỐ ĐIỆN THOẠI"
              />
              <!-- Icon cảnh báo đỏ (chỉ hiện khi có lỗi) -->
              <div v-if="errors.email" class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none">
                <svg class="h-5 w-5 text-red-500" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                  <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
                </svg>
              </div>
            </div>
            <!-- Dòng chữ báo lỗi -->
            <p v-if="errors.email" class="mt-1 text-[10px] text-red-600 font-bold animate-fade-in">
              {{ errors.email }}
            </p>
          </div>

          <!-- Input Password -->
          <div>
            <div class="relative">
              <input 
                v-model="formData.password"
                type="password" 
                :class="[
                  'w-full h-10 px-3 border text-xs font-bold focus:outline-none focus:ring-1 transition-all rounded-none pr-10',
                  errors.password 
                    ? 'border-red-500 text-red-900 placeholder-red-300 focus:border-red-500 focus:ring-red-500' 
                    : 'border-black bg-white text-black placeholder-gray-400 focus:ring-black'
                ]"
                placeholder="MẬT KHẨU"
              />
              <!-- Icon cảnh báo đỏ -->
              <div v-if="errors.password" class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none">
                <svg class="h-5 w-5 text-red-500" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                  <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
                </svg>
              </div>
            </div>
            <!-- Dòng chữ báo lỗi -->
            <p v-if="errors.password" class="mt-1 text-[10px] text-red-600 font-bold animate-fade-in">
              {{ errors.password }}
            </p>

            <div class="flex justify-end mt-1.5">
              <NuxtLink to="/forgot-password" class="text-[10px] font-bold text-gray-500 underline hover:text-black uppercase tracking-wide">
                Quên mật khẩu?
              </NuxtLink>
            </div>
          </div>

          <!-- Nút Đăng Nhập -->
          <button 
            type="submit"
            :disabled="isLoading"
            class="group w-full h-10 bg-black text-white flex items-center justify-between px-4 hover:bg-gray-800 transition-colors duration-300 disabled:opacity-70 disabled:cursor-not-allowed"
          >
            <span v-if="!isLoading" class="text-xs font-bold uppercase tracking-widest">ĐĂNG NHẬP</span>
            <span v-else class="text-xs font-bold uppercase tracking-widest">ĐANG XỬ LÝ...</span>
            
            <svg v-if="!isLoading" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4 transform group-hover:translate-x-1 transition-transform">
              <path stroke-linecap="round" stroke-linejoin="round" d="M13.5 4.5L21 12m0 0l-7.5 7.5M21 12H3" />
            </svg>
            <svg v-else class="animate-spin h-4 w-4 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
              <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
              <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
            </svg>
          </button>

          <!-- Divider -->
          <div class="flex items-center gap-3 py-1">
            <div class="h-px bg-gray-200 flex-1"></div>
            <span class="text-[10px] font-bold text-gray-400 uppercase">Hoặc</span>
            <div class="h-px bg-gray-200 flex-1"></div>
          </div>

          <!-- Social Login Buttons -->
          <div class="grid grid-cols-2 gap-3">
            <button type="button" class="h-10 border border-gray-300 flex items-center justify-center gap-2 hover:border-black transition group">
              <svg class="w-4 h-4 grayscale group-hover:grayscale-0 transition" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M21.8055 10.0415H21V10H12V14H17.6515C16.827 16.3285 14.6115 18 12 18C8.6865 18 6 15.3135 6 12C6 8.6865 8.6865 6 12 6C13.5295 6 14.921 6.577 15.9805 7.5195L18.809 4.691C17.023 3.0265 14.634 2 12 2C6.4775 2 2 6.4775 2 12C2 17.5225 6.4775 22 12 22C17.5225 22 22 17.5225 22 12C22 11.3295 21.931 10.675 21.8055 10.0415Z" fill="#FFC107"/>
                <path d="M3.15295 7.3455L6.4385 9.755C7.3275 7.554 9.4805 6 12 6C13.5295 6 14.921 6.577 15.9805 7.5195L18.809 4.691C17.023 3.0265 14.634 2 12 2C8.1585 2 4.828 4.1685 3.15295 7.3455Z" fill="#FF3D00"/>
                <path d="M12 22C14.664 22 17.081 20.948 18.8765 19.247L15.543 16.7065C14.549 17.518 13.3255 18 12 18C9.3585 18 7.1185 16.2875 6.3055 13.923L3.0635 16.4085C4.7335 19.7435 8.106 22 12 22Z" fill="#4CAF50"/>
                <path d="M21.8055 10.0415H21V10H12V14H17.6515C17.257 15.108 16.546 16.073 15.543 16.7065L18.8765 19.247C19.279 18.8775 21.464 16.5865 21.8055 10.0415Z" fill="#1976D2"/>
              </svg>
              <span class="text-[10px] font-bold uppercase tracking-wider">Google</span>
            </button>
            <button type="button" class="h-10 border border-gray-300 flex items-center justify-center gap-2 hover:border-black transition group">
              <svg class="w-4 h-4 text-[#1877F2] grayscale group-hover:grayscale-0 transition" fill="currentColor" viewBox="0 0 24 24">
                <path d="M9.101 23.691v-7.98H6.627v-3.667h2.474v-1.58c0-4.085 1.848-5.978 5.858-5.978.401 0 .955.042 1.468.103a8.68 8.68 0 0 1 1.141.195v3.325a8.623 8.623 0 0 0-.653-.036c-2.148 0-2.971.742-2.971 2.28v1.691h3.753l-.875 3.667h-2.878v7.99c.288.081.58.154.875.219V24h-.012c-.852.154-1.729.242-2.625.242-.831 0-1.645-.076-2.437-.214v-.337z"/>
              </svg>
              <span class="text-[10px] font-bold uppercase tracking-wider">Facebook</span>
            </button>
          </div>

        </form>

        <!-- Register Link -->
        <div class="mt-8 pt-4 border-t border-gray-100">
          <p class="text-xs font-bold text-black uppercase mb-2">BẠN CHƯA CÓ TÀI KHOẢN?</p>
          <NuxtLink to="/register" class="group w-full h-10 border border-black flex items-center justify-between px-4 hover:bg-black hover:text-white transition-colors duration-300">
            <span class="text-xs font-bold uppercase tracking-widest">ĐĂNG KÝ NGAY</span>
            <i class="fa-solid fa-arrow-right transform group-hover:translate-x-1 transition-transform text-sm"></i>
          </NuxtLink>
        </div>

      </div>
    </div>

  </div>
</template>

<style scoped>
.animate-fade-in-up {
  animation: fadeInUp 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
  opacity: 0;
  transform: translateY(20px);
}

.animate-fade-in {
  animation: fadeIn 1s ease-out forwards;
  opacity: 0;
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
</style>