<script setup>
definePageMeta({
  layout: 'default'
})

const router = useRouter()

// --- STATE (Mock Data) ---
const customerInfo = reactive({
  fullName: 'NGUYỄN VĂN A',
  email: 'nguyenvana@example.com',
  phone: '0912345678',
  address: '',
  city: '',
  note: ''
})

const orderItems = ref([
  {
    id: 1,
    name: 'GIÀY FORUM LOW CL',
    size: 41,
    price: 2600000,
    quantity: 1,
    image: 'https://placehold.co/150x150/f3f4f6/000000?text=Forum'
  },
  {
    id: 2,
    name: 'ÁO KHOÁC GIÓ ADICOLOR',
    size: 'L',
    price: 1800000,
    quantity: 2,
    image: 'https://placehold.co/150x150/f3f4f6/000000?text=Jacket'
  }
])

const paymentMethod = ref('cod')

// --- COMPUTED ---
const subtotal = computed(() => {
  return orderItems.value.reduce((sum, item) => sum + (item.price * item.quantity), 0)
})

const shippingFee = computed(() => {
  return subtotal.value > 3000000 ? 0 : 30000
})

const total = computed(() => {
  return subtotal.value + shippingFee.value
})

// --- METHODS ---
const formatCurrency = (amount) => {
  return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount)
}

const handleOrder = () => {
  alert('Đặt hàng thành công!')
  // navigateTo('/success')
}
</script>

<template>
  <div class="min-h-screen bg-white font-inter text-black pb-20">
    
    <!-- 1. HEADER PAGE -->
    <div class="container mx-auto px-6 pt-12 pb-8 border-b border-gray-200">
      <h1 class="text-4xl font-black uppercase tracking-tighter mb-2">THANH TOÁN</h1>
      <div class="flex items-center gap-2 text-xs font-bold uppercase tracking-widest text-gray-500">
        <span>Giỏ hàng</span>
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-3 h-3">
          <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 4.5l7.5 7.5-7.5 7.5" />
        </svg>
        <span class="text-black border-b border-black">Thanh toán</span>
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-3 h-3">
          <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 4.5l7.5 7.5-7.5 7.5" />
        </svg>
        <span>Hoàn tất</span>
      </div>
    </div>

    <div class="container mx-auto px-6 mt-12">
      <div class="flex flex-col lg:flex-row gap-12">
        
        <!-- === CỘT TRÁI: FORM THÔNG TIN === -->
        <div class="w-full lg:w-3/5 space-y-12">
          
          <!-- 1. Delivery Info -->
          <div>
            <h3 class="font-black text-xl uppercase tracking-tighter mb-6 flex items-center gap-2">
              1. THÔNG TIN GIAO HÀNG
            </h3>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <!-- Họ tên -->
              <div class="md:col-span-2">
                <input 
                  v-model="customerInfo.fullName" 
                  type="text" 
                  class="w-full h-12 px-4 border border-gray-300 text-sm font-bold placeholder-gray-400 focus:border-black focus:outline-none rounded-none bg-white transition-colors" 
                  placeholder="HỌ VÀ TÊN *"
                >
              </div>

              <!-- Email -->
              <div>
                <input 
                  v-model="customerInfo.email" 
                  type="email" 
                  class="w-full h-12 px-4 border border-gray-300 text-sm font-bold placeholder-gray-400 focus:border-black focus:outline-none rounded-none bg-white transition-colors" 
                  placeholder="EMAIL *"
                >
              </div>

              <!-- SĐT -->
              <div>
                <input 
                  v-model="customerInfo.phone" 
                  type="tel" 
                  class="w-full h-12 px-4 border border-gray-300 text-sm font-bold placeholder-gray-400 focus:border-black focus:outline-none rounded-none bg-white transition-colors" 
                  placeholder="SỐ ĐIỆN THOẠI *"
                >
              </div>

              <!-- Tỉnh/Thành -->
              <div>
                <div class="relative">
                  <select v-model="customerInfo.city" class="w-full h-12 px-4 border border-gray-300 text-sm font-bold text-gray-500 focus:border-black focus:outline-none rounded-none bg-white appearance-none cursor-pointer">
                    <option value="" disabled>CHỌN TỈNH / THÀNH PHỐ *</option>
                    <option value="HCM">HỒ CHÍ MINH</option>
                    <option value="HN">HÀ NỘI</option>
                    <option value="DN">ĐÀ NẴNG</option>
                  </select>
                  <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4 absolute right-4 top-1/2 -translate-y-1/2 pointer-events-none">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 8.25l-7.5 7.5-7.5-7.5" />
                  </svg>
                </div>
              </div>

              <!-- Địa chỉ -->
              <div>
                <input 
                  v-model="customerInfo.address" 
                  type="text" 
                  class="w-full h-12 px-4 border border-gray-300 text-sm font-bold placeholder-gray-400 focus:border-black focus:outline-none rounded-none bg-white transition-colors" 
                  placeholder="ĐỊA CHỈ (SỐ NHÀ, ĐƯỜNG) *"
                >
              </div>

              <!-- Ghi chú -->
              <div class="md:col-span-2">
                <textarea 
                  v-model="customerInfo.note" 
                  rows="3" 
                  class="w-full p-4 border border-gray-300 text-sm font-bold placeholder-gray-400 focus:border-black focus:outline-none rounded-none bg-white transition-colors" 
                  placeholder="GHI CHÚ ĐƠN HÀNG (TÙY CHỌN)"
                ></textarea>
              </div>
            </div>
          </div>

          <!-- 2. Payment Method -->
          <div>
            <h3 class="font-black text-xl uppercase tracking-tighter mb-6 flex items-center gap-2">
              2. PHƯƠNG THỨC THANH TOÁN
            </h3>

            <div class="space-y-4">
              <!-- COD -->
              <label 
                class="flex items-center justify-between p-6 border cursor-pointer transition-all duration-200 group hover:bg-gray-50"
                :class="paymentMethod === 'cod' ? 'border-black bg-gray-50' : 'border-gray-300'"
              >
                <div class="flex items-center gap-4">
                  <div class="relative flex items-center justify-center">
                    <input type="radio" value="cod" v-model="paymentMethod" class="peer appearance-none w-5 h-5 border-2 border-black rounded-full checked:bg-black checked:border-black transition-colors">
                    <div class="absolute w-2 h-2 bg-white rounded-full opacity-0 peer-checked:opacity-100 pointer-events-none"></div>
                  </div>
                  <div>
                    <p class="font-bold text-sm uppercase tracking-wide">THANH TOÁN KHI NHẬN HÀNG (COD)</p>
                    <p class="text-xs text-gray-500 mt-1 font-medium">Bạn chỉ thanh toán khi nhận được hàng.</p>
                  </div>
                </div>
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 text-gray-400 group-hover:text-black transition-colors">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M2.25 18.75a60.07 60.07 0 0115.797 2.101c.727.198 1.453-.342 1.453-1.096V18.75M3.75 4.5v.75A.75.75 0 013 6h-.75m0 0v-.375c0-.621.504-1.125 1.125-1.125H20.25M2.25 6v9m18-10.5v.75c0 .414.336.75.75.75h.75m-1.5-1.5h.375c.621 0 1.125.504 1.125 1.125v9.75c0 .621-.504 1.125-1.125 1.125h-.375m1.5-1.5H21a.75.75 0 00-.75.75v.75m0 0H3.75m0 0h-.375a1.125 1.125 0 01-1.125-1.125V15m1.5 1.5v-.75A.75.75 0 003 15h-.75M15 10.5a3 3 0 11-6 0 3 3 0 016 0zm3 0h.008v.008H18V10.5zm-12 0h.008v.008H6V10.5z" />
                </svg>
              </label>

              <!-- VNPAY -->
              <label 
                class="flex items-center justify-between p-6 border cursor-pointer transition-all duration-200 group hover:bg-gray-50"
                :class="paymentMethod === 'vnpay' ? 'border-black bg-gray-50' : 'border-gray-300'"
              >
                <div class="flex items-center gap-4">
                  <div class="relative flex items-center justify-center">
                    <input type="radio" value="vnpay" v-model="paymentMethod" class="peer appearance-none w-5 h-5 border-2 border-black rounded-full checked:bg-black checked:border-black transition-colors">
                    <div class="absolute w-2 h-2 bg-white rounded-full opacity-0 peer-checked:opacity-100 pointer-events-none"></div>
                  </div>
                  <div>
                    <p class="font-bold text-sm uppercase tracking-wide text-blue-700">THANH TOÁN QUA VNPAY</p>
                    <p class="text-xs text-gray-500 mt-1 font-medium">Quét mã QR hoặc dùng thẻ ATM/Visa nội địa.</p>
                  </div>
                </div>
                <img src="https://thuonghieumanh.vneconomy.vn/upload/vnpay.png" alt="VNPay" width="80px">
              </label>
            </div>
          </div>

        </div>

        <!-- === CỘT PHẢI: TÓM TẮT ĐƠN HÀNG === -->
        <div class="w-full lg:w-2/5">
          <div class="bg-gray-50 border border-black p-8 sticky top-24">
            <h3 class="font-black text-xl uppercase tracking-tighter mb-6 border-b border-gray-200 pb-4">ĐƠN HÀNG CỦA BẠN</h3>

            <!-- List Items -->
            <div class="space-y-6 mb-8 max-h-[300px] overflow-y-auto pr-2 custom-scrollbar">
              <div v-for="item in orderItems" :key="item.id" class="flex gap-4 items-start">
                <div class="relative w-20 h-20 bg-white border border-gray-200 flex-shrink-0">
                  <img :src="item.image" class="w-full h-full object-cover p-1">
                  <span class="absolute -top-2 -right-2 bg-black text-white text-[10px] w-5 h-5 flex items-center justify-center font-bold rounded-none">
                    {{ item.quantity }}
                  </span>
                </div>
                <div class="flex-1">
                  <h4 class="text-xs font-bold text-black uppercase tracking-wide leading-tight mb-1">{{ item.name }}</h4>
                  <p class="text-[10px] font-bold text-gray-500 uppercase tracking-wider">SIZE: {{ item.size }}</p>
                </div>
                <div class="text-sm font-bold text-black">
                  {{ formatCurrency(item.price * item.quantity) }}
                </div>
              </div>
            </div>

            <!-- Calculations -->
            <div class="space-y-3 mb-6 pt-6 border-t border-gray-200 text-sm font-medium">
              <div class="flex justify-between">
                <span class="text-gray-600 uppercase text-xs tracking-wide">TẠM TÍNH</span>
                <span class="font-bold">{{ formatCurrency(subtotal) }}</span>
              </div>
              <div class="flex justify-between">
                <span class="text-gray-600 uppercase text-xs tracking-wide">PHÍ VẬN CHUYỂN</span>
                <span v-if="shippingFee === 0" class="text-black font-black text-xs uppercase">MIỄN PHÍ</span>
                <span v-else class="font-bold">{{ formatCurrency(shippingFee) }}</span>
              </div>
            </div>

            <!-- Total -->
            <div class="flex justify-between items-end mb-8 pt-6 border-t border-black">
              <span class="font-black text-sm uppercase tracking-widest">TỔNG CỘNG</span>
              <div class="text-right">
                <span class="text-[10px] text-gray-500 font-bold uppercase tracking-wide block mb-1">BAO GỒM VAT</span>
                <span class="font-black text-2xl tracking-tight">{{ formatCurrency(total) }}</span>
              </div>
            </div>

            <!-- Order Button -->
            <button 
              @click="handleOrder"
              class="group w-full h-14 bg-black text-white flex items-center justify-between px-6 hover:bg-white hover:text-black border border-black transition-all duration-300"
            >
              <span class="text-sm font-bold uppercase tracking-widest">ĐẶT HÀNG NGAY</span>
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-5 h-5 transform group-hover:translate-x-1 transition-transform">
                <path stroke-linecap="round" stroke-linejoin="round" d="M13.5 4.5L21 12m0 0l-7.5 7.5M21 12H3" />
              </svg>
            </button>

            <div class="mt-4 text-center">
              <NuxtLink to="/cart" class="text-[10px] font-bold text-gray-500 uppercase tracking-widest underline hover:text-black transition">
                QUAY LẠI GIỎ HÀNG
              </NuxtLink>
            </div>

          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<style scoped>
/* Custom Scrollbar Minimalist */
.custom-scrollbar::-webkit-scrollbar {
  width: 3px;
}
.custom-scrollbar::-webkit-scrollbar-track {
  background: transparent; 
}
.custom-scrollbar::-webkit-scrollbar-thumb {
  background: #000; 
}
.custom-scrollbar::-webkit-scrollbar-thumb:hover {
  background: #333; 
}
</style>