<script setup>
definePageMeta({
  layout: 'default'
})

// --- MOCK DATA (DỮ LIỆU GIẢ) ---
const product = ref({
  id: 1,
  name: 'GIÀY FORUM LOW CL',
  price: 2600000,
  originalPrice: 3200000,
  description: 'Dòng giày Forum Low kinh điển với thiết kế vượt thời gian. Chất liệu da cao cấp kết hợp cùng đế cao su bền bỉ, mang lại cảm giác thoải mái tối đa cho người sử dụng. Phù hợp cho cả đi chơi và hoạt động thể thao nhẹ.',
  sku: 'BW-001',
  category: 'NAM ORIGINALS',
  tags: ['Vintage', 'Low Top', 'Streetwear'],
  sizes: [39, 40, 41, 42, 43],
  // Mới thêm: Colors
  colors: [
    { id: 'c1', name: 'Cloud White', hex: '#F3F4F6', class: 'bg-gray-100' },
    { id: 'c2', name: 'Core Black', hex: '#000000', class: 'bg-black' },
    { id: 'c3', name: 'Royal Blue', hex: '#1e40af', class: 'bg-blue-800' }
  ],
  images: [
    'https://placehold.co/600x600/f3f4f6/000000?text=Main+View',
    'https://placehold.co/600x600/f3f4f6/000000?text=Side+View',
    'https://placehold.co/600x600/f3f4f6/000000?text=Back+View',
    'https://placehold.co/600x600/f3f4f6/000000?text=Detail+View'
  ],
  reviews: [
    { id: 1, user: 'Nguyễn Văn A', rating: 5, date: '2 ngày trước', content: 'Giày đẹp, form chuẩn, giao hàng nhanh.', verified: true },
    { id: 2, user: 'Lê Thị B', rating: 4, date: '1 tuần trước', content: 'Chất lượng tốt trong tầm giá. Đóng gói cẩn thận.', verified: true }
  ],
  // Mới thêm: Comments (Hỏi đáp/Thảo luận)
  comments: [
    { id: 1, user: 'Trần Văn C', date: 'Hôm qua', content: 'Shop ơi mẫu này còn size 44 không ạ?', reply: 'Chào bạn, hiện tại mẫu này bên mình chỉ còn đến size 43 thôi ạ.' },
    { id: 2, user: 'Phạm D', date: '3 giờ trước', content: 'Ship hỏa tốc nội thành HCM mất bao lâu?' }
  ]
})

const relatedProducts = ref([
  { id: 102, name: 'ÁO KHOÁC GIÓ ADICOLOR', price: 1800000, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Jacket' },
  { id: 103, name: 'TÚI ĐEO CHÉO ESSENTIAL', price: 450000, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Bag' },
  { id: 104, name: 'MŨ LƯỠI TRAI BASEBALL', price: 350000, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Cap' },
  { id: 105, name: 'TẤT CỔ CAO (3 ĐÔI)', price: 150000, image: 'https://placehold.co/500x500/f3f4f6/000000?text=Socks' }
])

// --- STATE ---
const mainImage = ref(product.value.images[0])
const quantity = ref(1)
const activeTab = ref('description')
const selectedSize = ref(null)
// Mới thêm: State cho màu sắc
const selectedColor = ref(product.value.colors[0]) 
// Mới thêm: State cho input bình luận
const newCommentContent = ref('')

// --- METHODS ---
const setMainImage = (img) => {
  mainImage.value = img
}

const updateQuantity = (change) => {
  const newVal = quantity.value + change
  if (newVal >= 1) quantity.value = newVal
}

const formatCurrency = (amount) => {
  return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount)
}

const addToCart = () => {
  if (!selectedSize.value) {
    alert('Vui lòng chọn size!')
    return
  }
  // Cập nhật thông báo thêm màu sắc
  alert(`Đã thêm ${product.value.name}\n- Size: ${selectedSize.value}\n- Màu: ${selectedColor.value.name}\nvào giỏ hàng!`)
}

const submitComment = () => {
  if(!newCommentContent.value.trim()) return;
  
  product.value.comments.unshift({
    id: Date.now(),
    user: 'Khách hàng',
    date: 'Vừa xong',
    content: newCommentContent.value,
    reply: null
  })
  newCommentContent.value = ''
  alert('Đã gửi bình luận của bạn!')
}
</script>

<template>
  <div class="min-h-screen bg-white font-inter text-black pb-20">
    
    <!-- 1. BREADCRUMB -->
    <div class="container mx-auto px-6 py-6">
      <div class="flex items-center gap-2 text-xs font-bold uppercase tracking-widest text-gray-500">
        <NuxtLink to="/" class="hover:text-black transition">Trang chủ</NuxtLink>
        <span>/</span>
        <NuxtLink to="/shop" class="hover:text-black transition">Cửa hàng</NuxtLink>
        <span>/</span>
        <span class="text-black border-b border-black">{{ product.name }}</span>
      </div>
    </div>

    <!-- 2. MAIN CONTENT GRID -->
    <div class="container mx-auto px-6">
      <div class="flex flex-col lg:flex-row gap-12 mb-20">
        
        <!-- LEFT: IMAGE GALLERY -->
        <div class="w-full lg:w-7/12">
          <!-- Ảnh Chính (Square) -->
          <div class="relative aspect-square bg-gray-100 mb-4 cursor-zoom-in group overflow-hidden">
            <img 
              :src="mainImage" 
              class="w-full h-full object-cover transition duration-700 group-hover:scale-105" 
              alt="Product Image"
            >
            <div class="absolute top-4 left-4 bg-black text-white px-3 py-1 text-xs font-bold uppercase tracking-widest">
              Mới
            </div>
          </div>

          <!-- Thumbnails (4 cột) -->
          <div class="grid grid-cols-4 gap-4">
            <div 
              v-for="(img, idx) in product.images" 
              :key="idx"
              @click="setMainImage(img)"
              class="aspect-square bg-gray-100 cursor-pointer border-2 transition hover:opacity-100 relative"
              :class="mainImage === img ? 'border-black opacity-100' : 'border-transparent opacity-60'"
            >
              <img :src="img" class="w-full h-full object-cover">
              <!-- Overlay active effect -->
              <div v-if="mainImage === img" class="absolute inset-0 border-b-4 border-black pointer-events-none"></div>
            </div>
          </div>
        </div>

        <!-- RIGHT: PRODUCT INFO -->
        <div class="w-full lg:w-5/12 flex flex-col">
          
          <!-- Category & Name -->
          <div class="mb-6">
            <div class="flex justify-between items-start">
              <p class="text-xs font-bold text-gray-500 uppercase tracking-widest mb-2">{{ product.category }}</p>
              <!-- Rating Stars -->
              <div class="flex items-center gap-1">
                <div class="flex text-black">
                  <svg v-for="i in 5" :key="i" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-3 h-3">
                    <path fill-rule="evenodd" d="M10.788 3.21c.448-1.077 1.976-1.077 2.424 0l2.082 5.007 5.404.433c1.164.093 1.636 1.545.749 2.305l-4.117 3.527 1.257 5.273c.271 1.136-.964 2.033-1.96 1.425L12 18.354 7.373 21.18c-.996.608-2.231-.29-1.96-1.425l1.257-5.273-4.117-3.527c-.887-.76-.415-2.212.749-2.305l5.404-.433 2.082-5.006z" clip-rule="evenodd" />
                  </svg>
                </div>
                <span class="text-xs font-bold text-black underline ml-1 cursor-pointer">{{ product.reviews.length }} đánh giá</span>
              </div>
            </div>
            
            <h1 class="text-4xl lg:text-5xl font-black uppercase tracking-tighter leading-tight mb-4">
              {{ product.name }}
            </h1>
            
            <div class="flex items-center gap-4">
              <span class="text-2xl font-bold">{{ formatCurrency(product.price) }}</span>
              <span v-if="product.originalPrice" class="text-lg text-gray-400 line-through font-medium">{{ formatCurrency(product.originalPrice) }}</span>
            </div>
          </div>

          <!-- Short Description -->
          <div class="mb-8">
            <p class="text-sm text-gray-600 leading-relaxed font-medium">
              {{ product.description }}
            </p>
          </div>

          <!-- Size Selection -->
          <div class="mb-6">
            <div class="flex justify-between mb-3">
              <span class="text-xs font-bold uppercase tracking-widest">Chọn Size</span>
              <button class="text-xs font-bold uppercase tracking-widest underline hover:text-gray-600">Hướng dẫn size</button>
            </div>
            <div class="grid grid-cols-5 gap-2">
              <button 
                v-for="size in product.sizes" 
                :key="size"
                @click="selectedSize = size"
                class="h-12 flex items-center justify-center border text-sm font-bold transition-all duration-200 hover:border-black"
                :class="selectedSize === size ? 'bg-black text-white border-black' : 'bg-white text-black border-gray-200'"
              >
                {{ size }}
              </button>
            </div>
          </div>

          <!-- MỚI THÊM: Color Selection -->
          <div class="mb-8">
             <div class="flex justify-between mb-3">
              <span class="text-xs font-bold uppercase tracking-widest">Màu sắc: <span class="text-gray-500 font-medium normal-case">{{ selectedColor.name }}</span></span>
            </div>
            <div class="flex gap-3">
              <button 
                v-for="color in product.colors" 
                :key="color.id"
                @click="selectedColor = color"
                class="w-10 h-10 rounded-full flex items-center justify-center border transition-all duration-200"
                :class="selectedColor.id === color.id ? 'border-black ring-1 ring-black ring-offset-2' : 'border-gray-200 hover:border-gray-400'"
              >
                <span class="w-8 h-8 rounded-full border border-gray-100 shadow-sm" :style="{ backgroundColor: color.hex }"></span>
              </button>
            </div>
          </div>

          <!-- Actions -->
          <div class="mt-[70px] space-y-4">
            
            <!-- Quantity & Add to Cart -->
            <div class="flex gap-4">
              <!-- Quantity -->
              <div class="flex w-32 h-14 border border-black">
                <button @click="updateQuantity(-1)" class="w-10 flex items-center justify-center hover:bg-gray-100 transition">
                  <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 12h-15" />
                  </svg>
                </button>
                <input type="text" :value="quantity" readonly class="w-full text-center font-bold focus:outline-none">
                <button @click="updateQuantity(1)" class="w-10 flex items-center justify-center hover:bg-gray-100 transition">
                  <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m7.5-7.5h-15" />
                  </svg>
                </button>
              </div>

              <!-- Add Button -->
              <button 
                @click="addToCart"
                class="flex-1 h-14 bg-black text-white flex items-center justify-between px-6 hover:bg-gray-800 transition group"
              >
                <span class="text-xs font-bold uppercase tracking-widest">Thêm vào giỏ</span>
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-5 h-5 transform group-hover:translate-x-1 transition-transform">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M13.5 4.5L21 12m0 0l-7.5 7.5M21 12H3" />
                </svg>
              </button>
            </div>

            <!-- Wishlist Button -->
            <button class="w-full h-12 border border-gray-300 flex items-center justify-center gap-2 hover:border-black transition text-xs font-bold uppercase tracking-widest">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-4 h-4">
                <path stroke-linecap="round" stroke-linejoin="round" d="M21 8.25c0-2.485-2.099-4.5-4.688-4.5-1.935 0-3.597 1.126-4.312 2.733-.715-1.607-2.377-2.733-4.313-2.733C5.1 3.75 3 5.765 3 8.25c0 7.22 9 12 9 12s9-4.78 9-12z" />
              </svg>
              Thêm vào yêu thích
            </button>

            <!-- Meta -->
            <div class="pt-6 border-t border-gray-100 grid grid-cols-2 gap-4 text-[10px] font-bold uppercase tracking-widest text-gray-500">
              <p>Mã SP: <span class="text-black">{{ product.sku }}</span></p>
              <p>Giao hàng & Đổi trả miễn phí</p>
            </div>

          </div>
        </div>
      </div>

      <!-- 3. TABS (DESCRIPTION & REVIEWS & COMMENTS) -->
      <div class="max-w-4xl mx-auto mb-24">
        <!-- Tab Buttons -->
        <div class="flex border-b border-gray-200 mb-8">
          <button 
            @click="activeTab = 'description'" 
            class="flex-1 py-4 text-sm font-bold uppercase tracking-widest border-b-2 transition-colors"
            :class="activeTab === 'description' ? 'border-black text-black' : 'border-transparent text-gray-400 hover:text-black'"
          >
            Mô tả
          </button>
          <button 
            @click="activeTab = 'reviews'" 
            class="flex-1 py-4 text-sm font-bold uppercase tracking-widest border-b-2 transition-colors"
            :class="activeTab === 'reviews' ? 'border-black text-black' : 'border-transparent text-gray-400 hover:text-black'"
          >
            Đánh giá ({{ product.reviews.length }})
          </button>
          <!-- Nút Bình luận mới -->
          <button 
            @click="activeTab = 'comments'" 
            class="flex-1 py-4 text-sm font-bold uppercase tracking-widest border-b-2 transition-colors"
            :class="activeTab === 'comments' ? 'border-black text-black' : 'border-transparent text-gray-400 hover:text-black'"
          >
            Bình luận ({{ product.comments.length }})
          </button>
        </div>

        <!-- Content Description -->
        <div v-if="activeTab === 'description'" class="animate-fade-in">
          <div class="prose max-w-none text-sm leading-relaxed text-gray-800">
            <p class="mb-4 font-bold">THÔNG SỐ SẢN PHẨM:</p>
            <ul class="list-disc pl-5 space-y-2 mb-6 marker:text-black">
              <li>Dáng regular fit</li>
              <li>Có dây giày</li>
              <li>Thân giày bằng da</li>
              <li>Lớp lót bằng vải dệt</li>
              <li>Đế ngoài bằng cao su</li>
              <li>Màu sản phẩm: {{ selectedColor.name }}</li>
            </ul>
            <p class="mb-4 font-bold">MÔ TẢ CHI TIẾT:</p>
            <p>{{ product.description }}</p>
            <p class="mt-4">Mang phong cách bóng rổ thập niên 80 trở lại đường phố. Đôi giày này trung thành với các chi tiết nguyên bản, từ quai cổ chân chữ X đến đế cupsole bằng cao su bền chắc. Hãy diện đôi giày này và cảm nhận tinh thần OG.</p>
          </div>
        </div>

        <!-- Content Reviews -->
        <div v-else-if="activeTab === 'reviews'" class="animate-fade-in space-y-8">
          <div v-for="review in product.reviews" :key="review.id" class="border-b border-gray-100 pb-8">
            <div class="flex justify-between items-start mb-3">
              <div class="flex items-center gap-4">
                <div class="w-12 h-12 bg-black text-white rounded-none flex items-center justify-center font-bold text-lg uppercase">
                  {{ review.user.charAt(0) }}
                </div>
                <div>
                  <p class="font-bold text-sm uppercase tracking-wide">{{ review.user }}</p>
                  <div class="flex text-black mt-1">
                    <svg v-for="s in 5" :key="s" :class="s <= review.rating ? 'fill-current' : 'text-gray-200 fill-current'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="w-3 h-3">
                      <path fill-rule="evenodd" d="M10.788 3.21c.448-1.077 1.976-1.077 2.424 0l2.082 5.007 5.404.433c1.164.093 1.636 1.545.749 2.305l-4.117 3.527 1.257 5.273c.271 1.136-.964 2.033-1.96 1.425L12 18.354 7.373 21.18c-.996.608-2.231-.29-1.96-1.425l1.257-5.273-4.117-3.527c-.887-.76-.415-2.212.749-2.305l5.404-.433 2.082-5.006z" clip-rule="evenodd" />
                    </svg>
                  </div>
                </div>
              </div>
              <span class="text-[10px] font-bold text-gray-400 uppercase tracking-wider">{{ review.date }}</span>
            </div>
            <p class="text-sm text-gray-700 leading-relaxed pl-16">{{ review.content }}</p>
            <div v-if="review.verified" class="mt-3 pl-16 flex items-center gap-1 text-[10px] font-bold text-black uppercase tracking-widest">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-3 h-3">
                <path fill-rule="evenodd" d="M2.25 12c0-5.385 4.365-9.75 9.75-9.75s9.75 4.365 9.75 9.75-4.365 9.75-9.75 9.75S2.25 17.385 2.25 12zm13.36-1.814a.75.75 0 10-1.22-.872l-3.236 4.53L9.53 12.22a.75.75 0 00-1.06 1.06l2.25 2.25a.75.75 0 001.14-.094l3.75-5.25z" clip-rule="evenodd" />
              </svg>
              Đã mua hàng
            </div>
          </div>
        </div>

        <!-- MỚI THÊM: Content Comments -->
        <div v-else-if="activeTab === 'comments'" class="animate-fade-in space-y-8">
          
          <!-- Box nhập comment -->
          <div class="bg-gray-50 p-6 mb-8">
            <h3 class="text-xs font-bold uppercase tracking-widest mb-4">Hỏi đáp & Thảo luận</h3>
            <textarea 
              v-model="newCommentContent"
              class="w-full border border-gray-200 p-4 text-sm focus:outline-none focus:border-black transition mb-4 min-h-[100px]"
              placeholder="Nhập câu hỏi hoặc thảo luận của bạn về sản phẩm..."
            ></textarea>
            <div class="flex justify-end">
               <button 
                @click="submitComment"
                class="bg-black text-white text-xs font-bold uppercase tracking-widest px-6 py-3 hover:bg-gray-800 transition"
              >
                Gửi bình luận
              </button>
            </div>
          </div>

          <!-- Danh sách comment -->
          <div v-for="comment in product.comments" :key="comment.id" class="border-b border-gray-100 pb-8 last:border-0">
            <div class="flex items-start gap-4">
              <!-- Avatar giả -->
              <div class="w-10 h-10 bg-gray-200 rounded-full flex items-center justify-center text-gray-500 font-bold text-xs uppercase">
                {{ comment.user.charAt(0) }}
              </div>
              <div class="flex-1">
                <div class="flex items-center justify-between mb-1">
                  <p class="font-bold text-sm uppercase tracking-wide">{{ comment.user }}</p>
                  <span class="text-[10px] font-bold text-gray-400 uppercase tracking-wider">{{ comment.date }}</span>
                </div>
                <p class="text-sm text-gray-700 leading-relaxed">{{ comment.content }}</p>
                
                <!-- Reply từ shop -->
                <div v-if="comment.reply" class="mt-4 bg-gray-50 p-4 border-l-2 border-black">
                  <div class="flex items-center gap-2 mb-1">
                    <span class="text-xs font-black uppercase">Admin Shop</span>
                    <span class="bg-black text-white text-[9px] px-1 font-bold uppercase">QTV</span>
                  </div>
                   <p class="text-sm text-gray-600">{{ comment.reply }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>

      </div>

      <!-- 4. RELATED PRODUCTS -->
      <div class="border-t border-gray-200 pt-12">
        <div class="flex justify-between items-end mb-8">
          <h2 class="text-2xl font-black uppercase tracking-tighter">CÓ THỂ BẠN SẼ THÍCH</h2>
          <NuxtLink to="/shop" class="text-xs font-bold uppercase tracking-widest underline hover:text-gray-600">Xem tất cả</NuxtLink>
        </div>

        <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
          <div v-for="rel in relatedProducts" :key="rel.id" class="group cursor-pointer">
            <div class="relative aspect-square bg-gray-100 mb-3 overflow-hidden">
              <img :src="rel.image" class="w-full h-full object-cover transition duration-700 group-hover:scale-105">
              <div class="absolute bottom-0 left-0 right-0 bg-white/90 py-2 px-4 translate-y-full group-hover:translate-y-0 transition-transform duration-300">
                <p class="text-[10px] font-bold uppercase tracking-widest text-center">Xem nhanh</p>
              </div>
            </div>
            <div class="flex justify-between items-start gap-2">
              <h3 class="text-xs font-bold uppercase tracking-wide truncate group-hover:underline">{{ rel.name }}</h3>
              <span class="text-xs font-medium">{{ formatCurrency(rel.price) }}</span>
            </div>
          </div>
        </div>
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