<script setup>
// 1. Ẩn Header/Footer mặc định
definePageMeta({
  layout: 'empty'
})

// --- STATE ---
const formData = ref({
  fullName: '',
  email: '',
  phone: '',
  password: '',
  confirmPassword: '',
  agree: false
})

// State lưu lỗi
const errors = ref({
  fullName: '',
  email: '',
  phone: '',
  password: '',
  confirmPassword: '',
  agree: ''
})

const isLoading = ref(false)

// --- VALIDATION LOGIC ---
const validateForm = () => {
  let isValid = true
  
  // Reset lỗi cũ
  Object.keys(errors.value).forEach(key => errors.value[key] = '')

  // 1. Họ tên
  if (!formData.value.fullName.trim()) {
    errors.value.fullName = 'Vui lòng nhập họ và tên'
    isValid = false
  }

  // 2. Email
  if (!formData.value.email.trim()) {
    errors.value.email = 'Vui lòng nhập email'
    isValid = false
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.value.email)) {
    errors.value.email = 'Email không hợp lệ'
    isValid = false
  }

  // 3. Số điện thoại
  if (!formData.value.phone.trim()) {
    errors.value.phone = 'Vui lòng nhập số điện thoại'
    isValid = false
  } else if (!/^\d{10,11}$/.test(formData.value.phone)) {
    errors.value.phone = 'Số điện thoại không hợp lệ (10-11 số)'
    isValid = false
  }

  // 4. Mật khẩu
  if (!formData.value.password) {
    errors.value.password = 'Vui lòng nhập mật khẩu'
    isValid = false
  } else if (formData.value.password.length < 6) {
    errors.value.password = 'Mật khẩu phải có ít nhất 6 ký tự'
    isValid = false
  }

  // 5. Xác nhận mật khẩu
  if (formData.value.confirmPassword !== formData.value.password) {
    errors.value.confirmPassword = 'Mật khẩu xác nhận không khớp'
    isValid = false
  }

  // 6. Điều khoản
  if (!formData.value.agree) {
    errors.value.agree = 'Bạn phải đồng ý với điều khoản dịch vụ'
    isValid = false
  }

  return isValid
}

const handleRegister = () => {
  if (!validateForm()) return

  isLoading.value = true

  // Giả lập gọi API đăng ký
  setTimeout(async () => {
    alert('Đăng ký thành công! Vui lòng đăng nhập.')
    await navigateTo('/login')
    isLoading.value = false
  }, 1500)
}
</script>

<template>
  <!-- Dùng h-screen và overflow-hidden để layout cố định -->
  <div class="h-screen flex font-inter text-black bg-white overflow-hidden">
    
    <!-- === CỘT TRÁI: BRAND INTRO (Giống hệt Login) === -->
    <div class="hidden lg:flex w-5/12 bg-black relative items-center justify-center">
      
      <!-- Background Pattern -->
      <div class="absolute inset-0 opacity-30 bg-[url('https://www.transparenttextures.com/patterns/cubes.png')]"></div>
      
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
          Gia nhập cộng đồng.<br>Trải nghiệm khác biệt.
        </p>

        <p class="mt-8 text-[10px] text-gray-500 uppercase tracking-widest animate-fade-in" style="animation-delay: 0.5s;">
          © 2025 BW Store.
        </p>
      </div>
    </div>

    <!-- === CỘT PHẢI: FORM ĐĂNG KÝ === -->
    <!-- Thêm overflow-y-auto vì form đăng ký dài hơn login -->
    <div class="w-full lg:w-7/12 flex flex-col justify-center p-8 lg:p-12 relative bg-white h-full overflow-y-auto">
      
      <!-- Nút Close -->
      <NuxtLink to="/" class="absolute top-6 left-6 text-gray-400 hover:text-black transition z-20">
        <span class="flex items-center gap-2 text-xs font-bold uppercase tracking-wider">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4">
            <path stroke-linecap="round" stroke-linejoin="round" d="M10.5 19.5L3 12m0 0l7.5-7.5M3 12h18" />
          </svg>
          Trang chủ
        </span>
      </NuxtLink>

      <div class="max-w-sm w-full mx-auto py-10">
        
        <!-- Tiêu đề -->
        <div class="mb-6">
          <h1 class="text-3xl font-black uppercase tracking-tighter mb-1 italic">TẠO TÀI KHOẢN</h1>
          <p class="text-xs text-gray-500 font-medium">Điền thông tin bên dưới để đăng ký thành viên.</p>
        </div>

        <!-- Form -->
        <form class="space-y-4" @submit.prevent="handleRegister">
          
          <!-- 1. Họ và tên -->
          <div>
            <div class="relative">
              <input 
                v-model="formData.fullName"
                type="text" 
                :class="[
                  'w-full h-10 px-3 border text-xs font-bold focus:outline-none focus:ring-1 transition-all rounded-none pr-10',
                  errors.fullName 
                    ? 'border-red-500 text-red-900 placeholder-red-300 focus:border-red-500 focus:ring-red-500' 
                    : 'border-black bg-white text-black placeholder-gray-400 focus:ring-black'
                ]"
                placeholder="HỌ VÀ TÊN"
              />
              <div v-if="errors.fullName" class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none">
                <svg class="h-4 w-4 text-red-500" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
                </svg>
              </div>
            </div>
            <p v-if="errors.fullName" class="mt-1 text-[10px] text-red-600 font-bold animate-fade-in">{{ errors.fullName }}</p>
          </div>

          <!-- 2. Email -->
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
                placeholder="EMAIL"
              />
              <div v-if="errors.email" class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none">
                <svg class="h-4 w-4 text-red-500" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
                </svg>
              </div>
            </div>
            <p v-if="errors.email" class="mt-1 text-[10px] text-red-600 font-bold animate-fade-in">{{ errors.email }}</p>
          </div>

          <!-- 3. Số điện thoại -->
          <div>
            <div class="relative">
              <input 
                v-model="formData.phone"
                type="tel" 
                :class="[
                  'w-full h-10 px-3 border text-xs font-bold focus:outline-none focus:ring-1 transition-all rounded-none pr-10',
                  errors.phone 
                    ? 'border-red-500 text-red-900 placeholder-red-300 focus:border-red-500 focus:ring-red-500' 
                    : 'border-black bg-white text-black placeholder-gray-400 focus:ring-black'
                ]"
                placeholder="SỐ ĐIỆN THOẠI"
              />
              <div v-if="errors.phone" class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none">
                <svg class="h-4 w-4 text-red-500" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
                </svg>
              </div>
            </div>
            <p v-if="errors.phone" class="mt-1 text-[10px] text-red-600 font-bold animate-fade-in">{{ errors.phone }}</p>
          </div>

          <!-- 4. Mật khẩu -->
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
              <div v-if="errors.password" class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none">
                <svg class="h-4 w-4 text-red-500" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
                </svg>
              </div>
            </div>
            <p v-if="errors.password" class="mt-1 text-[10px] text-red-600 font-bold animate-fade-in">{{ errors.password }}</p>
          </div>

          <!-- 5. Xác nhận mật khẩu -->
          <div>
            <div class="relative">
              <input 
                v-model="formData.confirmPassword"
                type="password" 
                :class="[
                  'w-full h-10 px-3 border text-xs font-bold focus:outline-none focus:ring-1 transition-all rounded-none pr-10',
                  errors.confirmPassword 
                    ? 'border-red-500 text-red-900 placeholder-red-300 focus:border-red-500 focus:ring-red-500' 
                    : 'border-black bg-white text-black placeholder-gray-400 focus:ring-black'
                ]"
                placeholder="XÁC NHẬN MẬT KHẨU"
              />
              <div v-if="errors.confirmPassword" class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none">
                <svg class="h-4 w-4 text-red-500" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
                </svg>
              </div>
            </div>
            <p v-if="errors.confirmPassword" class="mt-1 text-[10px] text-red-600 font-bold animate-fade-in">{{ errors.confirmPassword }}</p>
          </div>

          <!-- 6. Điều khoản -->
          <div>
            <div class="flex items-start">
              <input 
                id="agree" 
                type="checkbox" 
                v-model="formData.agree" 
                class="mt-0.5 w-4 h-4 text-black bg-gray-100 border-gray-300 rounded focus:ring-black focus:ring-2 accent-black cursor-pointer"
              >
              <label for="agree" class="ml-2 text-xs text-gray-600 cursor-pointer select-none">
                Tôi đồng ý với <a href="#" class="text-black font-bold hover:underline">Điều khoản dịch vụ</a> và <a href="#" class="text-black font-bold hover:underline">Chính sách bảo mật</a>.
              </label>
            </div>
            <p v-if="errors.agree" class="mt-1 text-[10px] text-red-600 font-bold animate-fade-in">{{ errors.agree }}</p>
          </div>

          <!-- Nút Đăng Ký -->
          <button 
            type="submit"
            :disabled="isLoading"
            class="group w-full h-10 bg-black text-white flex items-center justify-between px-4 hover:bg-gray-800 transition-colors duration-300 disabled:opacity-70 disabled:cursor-not-allowed mt-4"
          >
            <span v-if="!isLoading" class="text-xs font-bold uppercase tracking-widest">ĐĂNG KÝ NGAY</span>
            <span v-else class="text-xs font-bold uppercase tracking-widest">ĐANG XỬ LÝ...</span>
            
            <svg v-if="!isLoading" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4 transform group-hover:translate-x-1 transition-transform">
              <path stroke-linecap="round" stroke-linejoin="round" d="M13.5 4.5L21 12m0 0l-7.5 7.5M21 12H3" />
            </svg>
            <svg v-else class="animate-spin h-4 w-4 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
              <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
              <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
            </svg>
          </button>

        </form>

        <!-- Login Link -->
        <div class="mt-8 pt-4 border-t border-gray-100">
          <p class="text-xs font-bold text-black uppercase mb-2">ĐÃ CÓ TÀI KHOẢN?</p>
          <NuxtLink to="/login" class="group w-full h-10 border border-black flex items-center justify-between px-4 hover:bg-black hover:text-white transition-colors duration-300">
            <span class="text-xs font-bold uppercase tracking-widest">ĐĂNG NHẬP NGAY</span>
            <i class="fa-solid fa-arrow-right transform group-hover:translate-x-1 transition-transform text-sm"></i>
          </NuxtLink>
        </div>

      </div>
    </div>

  </div>
</template>

<style scoped>
/* Giữ nguyên Animation */
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