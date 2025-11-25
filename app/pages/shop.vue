<script setup>
definePageMeta({
  layout: 'default'
})

// --- MOCK DATA ---
const products = ref([
  { id: 1, name: 'GIÀY FORUM LOW CL', price: 2600000, originalPrice: 3200000, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Forum', isNew: true },
  { id: 2, name: 'ÁO KHOÁC GIÓ ADICOLOR', price: 1800000, originalPrice: null, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Jacket', isNew: false },
  { id: 3, name: 'TÚI ĐEO CHÉO ESSENTIAL', price: 450000, originalPrice: null, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Bag', isNew: false },
  { id: 4, name: 'MŨ LƯỠI TRAI BASEBALL', price: 350000, originalPrice: 500000, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Cap', isNew: false },
  { id: 5, name: 'QUẦN SHORT 3-STRIPES', price: 950000, originalPrice: null, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Shorts', isNew: true },
  { id: 6, name: 'ÁO THUN TREFOIL', price: 700000, originalPrice: null, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Tee', isNew: false },
  { id: 7, name: 'GIÀY SUPERSTAR XLG', price: 3100000, originalPrice: null, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Superstar', isNew: true },
  { id: 8, name: 'TẤT CỔ CAO (3 ĐÔI)', price: 150000, originalPrice: null, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Socks', isNew: false },
  { id: 9, name: 'DÉP ADILETTE 22', price: 1100000, originalPrice: 1400000, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Slides', isNew: false }
])

const categories = [
  { id: 'all', name: 'TẤT CẢ SẢN PHẨM' },
  { id: 'men', name: 'NAM ORIGINALS' },
  { id: 'women', name: 'NỮ ORIGINALS' },
  { id: 'accessories', name: 'PHỤ KIỆN' },
  { id: 'sale', name: 'GIẢM GIÁ' }
]

// --- STATE ---
const selectedCategory = ref('all')
const priceRange = ref(5000000)
const sortOption = ref('newest')
const currentPage = ref(1)
const totalPages = 5 

// --- METHODS ---
const formatCurrency = (amount) => {
  return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount)
}

const changePage = (page) => {
  if (page >= 1 && page <= totalPages) {
    currentPage.value = page
    window.scrollTo({ top: 0, behavior: 'smooth' })
  }
}
</script>

<template>
  <div class="min-h-screen bg-white font-inter text-black pb-20">
    
    <!-- 1. HEADER PAGE -->
    <div class="bg-white border-b border-gray-200 py-12">
      <div class="container mx-auto px-6 text-center">
        <h1 class="text-5xl font-black uppercase tracking-tighter mb-3">CỬA HÀNG</h1>
        <div class="flex justify-center items-center gap-2 text-xs font-bold uppercase tracking-widest text-gray-500">
          <NuxtLink to="/" class="hover:text-black transition">Trang chủ</NuxtLink>
          <span>/</span>
          <span class="text-black border-b border-black">Sản phẩm</span>
        </div>
      </div>
    </div>

    <div class="container mx-auto px-6 py-12">
      <div class="flex flex-col lg:flex-row gap-12">
        
        <!-- === SIDEBAR (BÊN TRÁI) === -->
        <aside class="w-full lg:w-1/4 space-y-12">
          
          <!-- Box 1: Tìm kiếm -->
          <div>
             <h3 class="text-sm font-black uppercase tracking-widest mb-4">TÌM KIẾM</h3>
             <div class="relative group">
               <input 
                 type="text" 
                 placeholder="TÌM SẢN PHẨM..." 
                 class="w-full h-12 px-4 border border-gray-300 text-xs font-bold placeholder-gray-400 focus:border-black focus:outline-none transition-colors rounded-none bg-white"
               >
               <button class="absolute right-0 top-0 h-12 w-12 flex items-center justify-center text-black hover:bg-gray-100 transition">
                 <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4">
                   <path stroke-linecap="round" stroke-linejoin="round" d="M21 21l-5.197-5.197m0 0A7.5 7.5 0 105.196 5.196a7.5 7.5 0 0010.607 10.607z" />
                 </svg>
               </button>
             </div>
          </div>

          <!-- Box 2: Danh Mục -->
          <div>
            <h3 class="text-sm font-black uppercase tracking-widest mb-4">DANH MỤC</h3>
            <ul class="space-y-1">
              <li v-for="cat in categories" :key="cat.id">
                <button 
                  @click="selectedCategory = cat.id"
                  class="flex items-center justify-between w-full text-left text-xs font-bold uppercase py-3 border-b border-transparent hover:border-gray-200 transition group"
                  :class="selectedCategory === cat.id ? 'text-black pl-2 border-l-4 border-l-black bg-gray-50' : 'text-gray-500 hover:text-black'"
                >
                  <span>{{ cat.name }}</span>
                  <svg v-if="selectedCategory === cat.id" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-3 h-3">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 4.5l7.5 7.5-7.5 7.5" />
                  </svg>
                </button>
              </li>
            </ul>
          </div>

          <!-- Box 3: Lọc Giá -->
          <div>
            <h3 class="text-sm font-black uppercase tracking-widest mb-6">KHOẢNG GIÁ</h3>
            <input 
              type="range" 
              min="0" 
              max="10000000" 
              step="100000"
              v-model="priceRange"
              class="w-full h-1 bg-gray-200 appearance-none cursor-pointer accent-black rounded-none"
            >
            <div class="flex justify-between mt-4 text-xs font-bold uppercase tracking-wider">
              <span class="text-gray-400">0 ₫</span>
              <span class="text-black">{{ formatCurrency(priceRange) }}</span>
            </div>
            <button class="w-full mt-6 h-12 border border-black bg-white text-black text-xs font-bold uppercase tracking-widest hover:bg-black hover:text-white transition-colors duration-300">
              ÁP DỤNG
            </button>
          </div>

        </aside>

        <!-- === MAIN CONTENT (BÊN PHẢI) === -->
        <main class="w-full lg:w-3/4">
          
          <!-- Toolbar -->
          <div class="flex flex-col sm:flex-row justify-between items-center mb-8 pb-6 border-b border-gray-100">
             <p class="text-xs font-bold text-gray-500 uppercase tracking-wider mb-4 sm:mb-0">
               HIỂN THỊ <span class="text-black">1–9</span> TRONG <span class="text-black">45</span> KẾT QUẢ
             </p>
             
             <div class="flex items-center gap-4">
               <label class="text-xs font-bold text-gray-500 uppercase tracking-wider hidden sm:block">SẮP XẾP:</label>
               <div class="relative">
                 <select v-model="sortOption" class="h-10 border border-gray-300 pl-4 pr-10 text-xs font-bold uppercase focus:border-black focus:outline-none cursor-pointer bg-white appearance-none rounded-none min-w-[180px]">
                   <option value="newest">MỚI NHẤT</option>
                   <option value="price_asc">GIÁ: THẤP ĐẾN CAO</option>
                   <option value="price_desc">GIÁ: CAO ĐẾN THẤP</option>
                   <option value="name_asc">TÊN: A-Z</option>
                 </select>
                 <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-3 h-3 absolute right-4 top-1/2 -translate-y-1/2 pointer-events-none">
                   <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 8.25l-7.5 7.5-7.5-7.5" />
                 </svg>
               </div>
             </div>
          </div>

          <!-- PRODUCT GRID -->
          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-x-6 gap-y-12 mb-16 animate-fade-in">
            
            <div v-for="product in products" :key="product.id" class="group cursor-pointer">
              
              <!-- Image Wrapper -->
              <div class="relative aspect-square bg-gray-100 mb-4 overflow-hidden">
                
                <!-- Badges -->
                <div class="absolute top-3 left-3 flex flex-col gap-2 z-10">
                  <span v-if="product.isNew" class="bg-white border border-black text-black text-[10px] font-bold px-2 py-1 uppercase tracking-wider">MỚI</span>
                  <span v-if="product.originalPrice" class="bg-black text-white text-[10px] font-bold px-2 py-1 uppercase tracking-wider">-20%</span>
                </div>

                <!-- Image (Link to Detail) -->
                <NuxtLink :to="`/product/${product.id}`">
                  <img 
                    :src="product.image" 
                    class="w-full h-full object-cover transition duration-700 group-hover:scale-105"
                  >
                </NuxtLink>

                <!-- Hover Actions (Desktop) -->
                <!-- Đã sửa thành XEM CHI TIẾT -->
                <NuxtLink 
                  :to="`/product/${product.id}`"
                  class="absolute inset-x-0 bottom-0 bg-white/90 py-3 px-4 translate-y-full group-hover:translate-y-0 transition-transform duration-300 flex justify-between items-center border-t border-black z-20"
                >
                  <span class="text-[10px] font-bold uppercase tracking-widest">XEM CHI TIẾT</span>
                  <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-4 h-4">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M13.5 4.5L21 12m0 0l-7.5 7.5M21 12H3" />
                  </svg>
                </NuxtLink>
              </div>

              <!-- Info -->
              <div>
                 <div class="flex justify-between items-start mb-1">
                   <h3 class="text-sm font-bold text-black uppercase tracking-tight group-hover:underline line-clamp-1 pr-4">
                     <NuxtLink :to="`/product/${product.id}`">{{ product.name }}</NuxtLink>
                   </h3>
                   <!-- Wishlist Icon Mini -->
                   <button class="text-gray-400 hover:text-black transition">
                     <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-4 h-4">
                       <path stroke-linecap="round" stroke-linejoin="round" d="M21 8.25c0-2.485-2.099-4.5-4.688-4.5-1.935 0-3.597 1.126-4.312 2.733-.715-1.607-2.377-2.733-4.313-2.733C5.1 3.75 3 5.765 3 8.25c0 7.22 9 12 9 12s9-4.78 9-12z" />
                     </svg>
                   </button>
                 </div>
                 
                 <div class="flex items-center gap-3">
                   <span class="text-sm font-medium text-black" :class="{'text-red-600': product.originalPrice}">{{ formatCurrency(product.price) }}</span>
                   <span v-if="product.originalPrice" class="text-xs text-gray-400 line-through font-medium">{{ formatCurrency(product.originalPrice) }}</span>
                 </div>
                 
                 <p class="text-[10px] text-gray-500 uppercase tracking-wider mt-1">NAM ORIGINALS</p>
              </div>

            </div>
          </div>

          <!-- Pagination -->
          <div class="flex justify-center gap-2">
            <!-- Prev -->
            <button 
              @click="changePage(currentPage - 1)"
              :disabled="currentPage === 1"
              class="w-10 h-10 flex items-center justify-center border border-gray-300 hover:border-black disabled:opacity-30 disabled:hover:border-gray-300 transition"
            >
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-3 h-3">
                <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5L8.25 12l7.5-7.5" />
              </svg>
            </button>

            <!-- Pages -->
            <button 
              v-for="page in totalPages" 
              :key="page"
              @click="changePage(page)"
              class="w-10 h-10 flex items-center justify-center border text-xs font-bold transition"
              :class="currentPage === page ? 'bg-black text-white border-black' : 'border-gray-300 hover:border-black text-black'"
            >
              {{ page }}
            </button>

            <!-- Next -->
            <button 
              @click="changePage(currentPage + 1)"
              :disabled="currentPage === totalPages"
              class="w-10 h-10 flex items-center justify-center border border-gray-300 hover:border-black disabled:opacity-30 disabled:hover:border-gray-300 transition"
            >
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-3 h-3">
                <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 4.5l7.5 7.5-7.5 7.5" />
              </svg>
            </button>
          </div>

        </main>
      </div>
    </div>
  </div>
</template>

<style scoped>
.animate-fade-in {
  animation: fadeIn 0.5s ease-out forwards;
}
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
</style>