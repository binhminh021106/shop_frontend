<script setup>
definePageMeta({
  layout: 'default'
})

const router = useRouter()

// --- STATE (Mock Data) ---
const cartItems = ref([
  {
    id: 1,
    name: 'GIÀY FORUM LOW CL',
    category: 'NAM ORIGINALS',
    price: 2600000,
    quantity: 1,
    size: 41,
    color: 'CLOUD WHITE',
    image: 'https://placehold.co/300x300/f3f4f6/000000?text=Forum'
  },
  {
    id: 2,
    name: 'ÁO KHOÁC GIÓ ADICOLOR',
    category: 'NAM THỜI TRANG',
    price: 1800000,
    quantity: 2,
    size: 'L',
    color: 'BLACK',
    image: 'https://placehold.co/300x300/f3f4f6/000000?text=Jacket'
  },
  {
    id: 4,
    name: 'MŨ LƯỠI TRAI BASEBALL',
    category: 'PHỤ KIỆN',
    price: 350000,
    quantity: 1,
    size: 'OSFM',
    color: 'NAVY',
    image: 'https://placehold.co/300x300/f3f4f6/000000?text=Cap'
  }
])

const promoCode = ref('')

// --- COMPUTED ---
const subtotal = computed(() => {
  return cartItems.value.reduce((sum, item) => sum + (item.price * item.quantity), 0)
})

const shippingFee = computed(() => {
  // Miễn phí ship cho đơn trên 3 triệu
  if (subtotal.value > 3000000) return 0
  return 30000
})

const total = computed(() => {
  return subtotal.value + shippingFee.value
})

// --- METHODS ---
const updateQuantity = (id, change) => {
  const item = cartItems.value.find(i => i.id === id)
  if (item) {
    const newQty = item.quantity + change
    if (newQty > 0) item.quantity = newQty
  }
}

const removeItem = (id) => {
  if (confirm('Bạn muốn xóa sản phẩm này?')) {
    cartItems.value = cartItems.value.filter(i => i.id !== id)
  }
}

const formatCurrency = (amount) => {
  return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount)
}

const handleCheckout = () => {
  alert('Chuyển hướng đến trang thanh toán...')
  // await navigateTo('/checkout')
}
</script>

<template>
  <div class="min-h-screen bg-white font-inter text-black pb-20">
    
    <!-- 1. HEADER PAGE -->
    <div class="container mx-auto px-6 pt-12 pb-8 border-b border-gray-200">
      <h1 class="text-4xl font-black uppercase tracking-tighter mb-2">
        GIỎ HÀNG CỦA BẠN
        <span class="text-gray-400 font-medium text-lg align-middle ml-2">({{ cartItems.length }} SẢN PHẨM)</span>
      </h1>
      <div class="flex items-center gap-2 text-xs font-bold uppercase tracking-widest text-gray-500">
        <span class="text-black border-b border-black">Giỏ hàng</span>
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-3 h-3">
          <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 4.5l7.5 7.5-7.5 7.5" />
        </svg>
        <span>Thanh toán</span>
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-3 h-3">
          <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 4.5l7.5 7.5-7.5 7.5" />
        </svg>
        <span>Hoàn tất</span>
      </div>
    </div>

    <div class="container mx-auto px-6 mt-12">
      
      <!-- EMPTY STATE -->
      <div v-if="cartItems.length === 0" class="py-20 text-center">
        <div class="mb-6 inline-flex items-center justify-center w-20 h-20 bg-gray-50 rounded-none">
           <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-8 h-8 text-gray-400">
             <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 10.5V6a3.75 3.75 0 10-7.5 0v4.5m11.356-1.993l1.263 12c.07.665-.45 1.243-1.119 1.243H4.25a1.125 1.125 0 01-1.12-1.243l1.264-12A1.125 1.125 0 015.513 7.5h12.974c.576 0 1.059.435 1.119 1.007zM8.625 10.5a.375.375 0 11-.75 0 .375.375 0 01.75 0zm7.5 0a.375.375 0 11-.75 0 .375.375 0 01.75 0z" />
           </svg>
        </div>
        <h2 class="text-2xl font-black uppercase tracking-tight mb-2">GIỎ HÀNG TRỐNG</h2>
        <p class="text-gray-500 mb-8 text-sm">Bạn chưa chọn sản phẩm nào.</p>
        <NuxtLink 
          to="/shop"
          class="group inline-flex h-12 items-center justify-center bg-black text-white px-8 text-xs font-bold uppercase tracking-widest hover:bg-gray-800 transition-all duration-300"
        >
          <span>QUAY LẠI CỬA HÀNG</span>
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4 ml-2 transform group-hover:translate-x-1 transition-transform">
            <path stroke-linecap="round" stroke-linejoin="round" d="M13.5 4.5L21 12m0 0l-7.5 7.5M21 12H3" />
          </svg>
        </NuxtLink>
      </div>

      <!-- CART CONTENT -->
      <div v-else class="flex flex-col lg:flex-row gap-12">
        
        <!-- 1. CART LIST (LEFT) -->
        <div class="w-full lg:w-2/3">
          
          <!-- Items -->
          <div class="space-y-8">
            <div 
              v-for="item in cartItems" 
              :key="item.id" 
              class="flex flex-col sm:flex-row gap-6 border-b border-gray-200 pb-8 last:border-0"
            >
              <!-- Image -->
              <div class="w-32 h-32 bg-gray-100 flex-shrink-0 relative overflow-hidden group cursor-pointer" @click="navigateTo(`/product/${item.id}`)">
                <img :src="item.image" class="w-full h-full object-cover transition duration-700 group-hover:scale-105">
              </div>

              <!-- Info -->
              <div class="flex-1 flex flex-col justify-between">
                <div>
                  <div class="flex justify-between items-start mb-1">
                    <p class="text-xs font-bold text-gray-500 uppercase tracking-widest">{{ item.category }}</p>
                    <span class="font-bold text-sm">{{ formatCurrency(item.price * item.quantity) }}</span>
                  </div>
                  <h3 
                    class="text-lg font-bold text-black uppercase tracking-tight hover:underline cursor-pointer mb-2"
                    @click="navigateTo(`/product/${item.id}`)"
                  >
                    {{ item.name }}
                  </h3>
                  <!-- Details -->
                  <div class="text-xs text-gray-600 space-y-1 font-medium uppercase tracking-wide">
                    <p>MÀU: <span class="text-black">{{ item.color }}</span></p>
                    <p>SIZE: <span class="text-black">{{ item.size }}</span></p>
                  </div>
                </div>

                <!-- Actions -->
                <div class="flex justify-between items-end mt-4 sm:mt-0">
                  <!-- Quantity Input -->
                  <div class="flex w-28 h-10 border border-gray-300">
                    <button @click="updateQuantity(item.id, -1)" class="w-8 flex items-center justify-center hover:bg-black hover:text-white transition font-bold text-lg">-</button>
                    <input type="text" :value="item.quantity" readonly class="w-full text-center font-bold text-sm focus:outline-none">
                    <button @click="updateQuantity(item.id, 1)" class="w-8 flex items-center justify-center hover:bg-black hover:text-white transition font-bold text-lg">+</button>
                  </div>

                  <!-- Remove Btn -->
                  <button 
                    @click="removeItem(item.id)" 
                    class="text-[10px] font-bold uppercase tracking-widest underline hover:text-gray-500 transition"
                  >
                    XÓA
                  </button>
                </div>
              </div>
            </div>
          </div>

          <!-- Back Btn -->
          <div class="mt-8 pt-8 border-t border-black">
            <NuxtLink to="/shop" class="inline-flex items-center gap-2 text-xs font-bold uppercase tracking-widest hover:underline">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4 transform rotate-180">
                <path stroke-linecap="round" stroke-linejoin="round" d="M13.5 4.5L21 12m0 0l-7.5 7.5M21 12H3" />
              </svg>
              TIẾP TỤC MUA SẮM
            </NuxtLink>
          </div>

        </div>

        <!-- 2. SUMMARY (RIGHT) -->
        <div class="w-full lg:w-1/3">
          <div class="bg-white border border-black p-8 sticky top-24">
            <h3 class="font-black text-xl uppercase tracking-tighter mb-6">TỔNG ĐƠN HÀNG</h3>
            
            <!-- Calc -->
            <div class="space-y-4 mb-8 pb-8 border-b border-gray-200">
              <div class="flex justify-between text-sm font-medium">
                <span class="text-gray-600 uppercase tracking-wide text-xs">TẠM TÍNH</span>
                <span class="font-bold">{{ formatCurrency(subtotal) }}</span>
              </div>
              <div class="flex justify-between text-sm font-medium">
                <span class="text-gray-600 uppercase tracking-wide text-xs">VẬN CHUYỂN</span>
                <span v-if="shippingFee === 0" class="text-black font-bold text-xs uppercase">MIỄN PHÍ</span>
                <span v-else class="font-bold">{{ formatCurrency(shippingFee) }}</span>
              </div>
            </div>

            <!-- Promo -->
            <div class="mb-8">
              <label class="text-xs font-bold uppercase tracking-widest mb-2 block">MÃ ƯU ĐÃI</label>
              <div class="flex h-12">
                <input 
                  v-model="promoCode"
                  type="text" 
                  placeholder="NHẬP MÃ" 
                  class="w-full border border-black border-r-0 px-4 text-sm font-bold uppercase placeholder-gray-400 focus:outline-none rounded-none"
                >
                <button class="bg-black text-white px-6 text-xs font-bold uppercase tracking-widest hover:bg-gray-800 transition">
                  ÁP DỤNG
                </button>
              </div>
            </div>

            <!-- Total -->
            <div class="flex justify-between items-end mb-8">
              <span class="font-black text-sm uppercase tracking-widest">TỔNG CỘNG</span>
              <div class="text-right">
                <span class="font-black text-2xl tracking-tight block">{{ formatCurrency(total) }}</span>
                <span class="text-[10px] text-gray-500 font-bold uppercase tracking-wide">(ĐÃ BAO GỒM VAT)</span>
              </div>
            </div>

            <!-- Checkout Btn -->
            <button 
              @click="handleCheckout"
              class="group w-full h-14 bg-black text-white flex items-center justify-between px-6 hover:bg-gray-800 transition-colors duration-300"
            >
              <span class="text-sm font-bold uppercase tracking-widest">THANH TOÁN</span>
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-5 h-5 transform group-hover:translate-x-1 transition-transform">
                <path stroke-linecap="round" stroke-linejoin="round" d="M13.5 4.5L21 12m0 0l-7.5 7.5M21 12H3" />
              </svg>
            </button>

            <!-- Cards -->
            <div class="mt-6 pt-6 border-t border-gray-100">
              <p class="text-[10px] font-bold text-gray-400 uppercase tracking-widest mb-3 text-center">CHÚNG TÔI CHẤP NHẬN</p>
              <div class="flex justify-center gap-4 opacity-50 grayscale">
                 <!-- Visa SVG -->
                 <svg class="h-6" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M32 10A14 14 0 1 0 32 38A14 14 0 1 0 32 10Z" fill="#2c3e50"/><path d="M16 10A14 14 0 1 0 16 38A14 14 0 1 0 16 10Z" fill="#ea4c89"/></svg>
                 <!-- Mastercard SVG -->
                 <svg class="h-6" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg"><rect width="32" height="32" rx="4" fill="#222"/><path d="M19.5 9.5h-7l-4 13h4l1-4h6l1 4h4l-5-13zm-4.5 7.5l1.5-5.5 1.5 5.5h-3z" fill="#fff"/></svg>
              </div>
            </div>

          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<style scoped>
/* Tùy chỉnh input number */
input[type=number]::-webkit-inner-spin-button, 
input[type=number]::-webkit-outer-spin-button { 
  -webkit-appearance: none; 
  margin: 0; 
}
</style>