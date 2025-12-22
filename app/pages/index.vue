<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

// ==================== STATE: SLIDER (HERO) ====================
const currentSlide = ref(0);
const slideInterval = ref(null);

const heroSlides = ref([
  {
    id: 1,
    image: 'https://images.unsplash.com/photo-1483985988355-763728e1935b?q=80&w=2070&auto=format&fit=crop',
    subtitle: 'Bộ Sưu Tập Hè 2025',
    title: 'PHONG CÁCH TỐI GIẢN',
    desc: 'Sự tinh tế đến từ những điều đơn giản nhất. Khám phá phong cách tối giản ngay hôm nay.',
    btnText: 'Khám Phá Ngay',
    link: '/shop',
    align: 'center' // center, left, right
  },
  {
    id: 2,
    image: 'https://images.unsplash.com/photo-1490481651871-ab68de25d43d?q=80&w=2070&auto=format&fit=crop',
    subtitle: 'Hàng Mới Về',
    title: 'THỜI TRANG ĐƯỜNG PHỐ',
    desc: 'Phong cách hiện đại, năng động và đầy cá tính cho giới trẻ.',
    btnText: 'Mua Sắm Ngay',
    link: '/shop',
    align: 'left'
  },
  {
    id: 3,
    image: 'https://images.unsplash.com/photo-1445205170230-053b83016050?q=80&w=2071&auto=format&fit=crop',
    subtitle: 'Ưu Đãi Độc Quyền',
    title: 'SALE MÙA ĐÔNG 50%',
    desc: 'Cơ hội sở hữu những món đồ yêu thích với mức giá tốt nhất năm.',
    btnText: 'Săn Sale Ngay',
    link: '/shop',
    align: 'right'
  }
]);

const nextSlide = () => {
  currentSlide.value = (currentSlide.value + 1) % heroSlides.value.length;
};

const prevSlide = () => {
  currentSlide.value = (currentSlide.value - 1 + heroSlides.value.length) % heroSlides.value.length;
};

const setSlide = (index) => {
  currentSlide.value = index;
  resetSlideTimer();
};

const startSlideTimer = () => {
  slideInterval.value = setInterval(nextSlide, 5000); // 5 giây đổi slide
};

const resetSlideTimer = () => {
  clearInterval(slideInterval.value);
  startSlideTimer();
};

// ==================== STATE: PRODUCTS ====================
const products = ref([]);
const isLoading = ref(false);
const currentPage = ref(1);
const totalPages = ref(0);
const limit = 5;

// MOCK DATA PRODUCTS
const allMockProducts = [
  { id: 1, name: 'ÁO THUN REGULAR FIT', price: 350000, description: 'Chất liệu cotton 100% thoáng mát.', image: 'https://placehold.co/400x500/f3f4f6/000000?text=Ao+Thun+1', status: 1 },
  { id: 2, name: 'QUẦN TÂY ỐNG ĐỨNG', price: 550000, description: 'Form dáng chuẩn công sở.', image: 'https://placehold.co/400x500/f3f4f6/000000?text=Quan+Tay', status: 1 },
  { id: 3, name: 'ÁO KHOÁC BOMBER', price: 850000, description: 'Thiết kế trẻ trung, giữ ấm tốt.', image: 'https://placehold.co/400x500/f3f4f6/000000?text=Bomber', status: 0 },
  { id: 4, name: 'GIÀY SNEAKER BASIC', price: 1200000, description: 'Đế cao su êm ái.', image: 'https://placehold.co/400x500/f3f4f6/000000?text=Sneaker', status: 1 },
  { id: 5, name: 'TÚI TOTE CANVAS', price: 150000, description: 'Đựng vừa laptop 15 inch.', image: 'https://placehold.co/400x500/f3f4f6/000000?text=Tui+Tote', status: 1 },
  { id: 6, name: 'MŨ LƯỠI TRAI DENIM', price: 220000, description: 'Chất liệu denim bụi bặm.', image: 'https://placehold.co/400x500/f3f4f6/000000?text=Mu+Luoi+Trai', status: 1 },
  { id: 7, name: 'ÁO POLO CỔ ĐIỂN', price: 420000, description: 'Vải cá sấu co giãn 4 chiều.', image: 'https://placehold.co/400x500/f3f4f6/000000?text=Polo', status: 0 },
  { id: 8, name: 'QUẦN SHORT KAKI', price: 320000, description: 'Lưng thun thoải mái vận động.', image: 'https://placehold.co/400x500/f3f4f6/000000?text=Short', status: 1 },
  { id: 9, name: 'ÁO HOODIE OVERSIZE', price: 650000, description: 'Nỉ bông dày dặn.', image: 'https://placehold.co/400x500/f3f4f6/000000?text=Hoodie', status: 1 },
  { id: 10, name: 'THẮT LƯNG DA THẬT', price: 500000, description: 'Da bò nguyên miếng.', image: 'https://placehold.co/400x500/f3f4f6/000000?text=Belt', status: 1 }
];

// CATEGORIES
const categories = ref([
  { name: 'Thời Trang Nam', count: 120, icon: 'fa-solid fa-shirt' },
  { name: 'Phụ Kiện', count: 45, icon: 'fa-regular fa-clock' },
  { name: 'Giày & Dép', count: 80, icon: 'fa-solid fa-shoe-prints' }
]);

// ==================== STATE: NEWS (TIN TỨC) ====================
const latestNews = ref([
  {
    id: 1,
    title: 'Xu Hướng Thời Trang Tối Giản 2025',
    date: '20/05/2025',
    excerpt: 'Minimalism không bao giờ lỗi thời. Cùng khám phá cách phối đồ đơn giản nhưng vẫn cuốn hút.',
    image: 'https://images.unsplash.com/photo-1485230946086-1d99d52666ad?q=80&w=2070&auto=format&fit=crop'
  },
  {
    id: 2,
    title: 'Cách Bảo Quản Áo Thun Luôn Như Mới',
    date: '18/05/2025',
    excerpt: 'Những mẹo nhỏ giúp chiếc áo thun yêu thích của bạn không bị bai dão hay phai màu sau nhiều lần giặt.',
    image: 'https://images.unsplash.com/photo-1523381210434-271e8be1f52b?q=80&w=2070&auto=format&fit=crop'
  },
  {
    id: 3,
    title: 'Bộ Sưu Tập Mùa Hè: Cảm Hứng Từ Biển Cả',
    date: '15/05/2025',
    excerpt: 'Gam màu xanh mát mắt và chất liệu linen thoáng khí là chủ đạo của bộ sưu tập lần này.',
    image: 'https://images.unsplash.com/photo-1515886657613-9f3515b0c78f?q=80&w=1020&auto=format&fit=crop'
  }
]);

// ==================== LOGIC CHUNG ====================
// Timer
const timer = ref({ hours: 0, minutes: 0, seconds: 0 });
let timerInterval = null;

const fetchProducts = async (page) => {
  isLoading.value = true;
  setTimeout(() => {
    const start = (page - 1) * limit;
    const end = start + limit;
    products.value = allMockProducts.slice(start, end);
    totalPages.value = Math.ceil(allMockProducts.length / limit);
    currentPage.value = page;
    isLoading.value = false;
  }, 500);
};

const changePage = (page) => {
  if (page >= 1 && page <= totalPages.value && !isLoading.value) {
    fetchProducts(page);
    document.getElementById('shop-section')?.scrollIntoView({ behavior: 'smooth', block: 'start' });
  }
};

const formatCurrency = (amount) => {
  return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount);
};

const goToDetail = (id) => {
  router.push(`/product/${id}`);
};

const startCountdown = () => {
  const endTime = new Date().getTime() + (2 * 60 * 60 * 1000 + 15 * 60 * 1000);
  timerInterval = setInterval(() => {
    const now = new Date().getTime();
    const distance = endTime - now;
    if (distance < 0) {
      clearInterval(timerInterval);
      return;
    }
    timer.value.hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    timer.value.minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
    timer.value.seconds = Math.floor((distance % (1000 * 60)) / 1000);
  }, 1000);
};

// Hooks
onMounted(() => {
  fetchProducts(1);
  startCountdown();
  startSlideTimer();
});

onUnmounted(() => {
  if (timerInterval) clearInterval(timerInterval);
  if (slideInterval.value) clearInterval(slideInterval.value);
});
</script>

<template>
  <div class="home-page pb-20 font-sans">

    <!-- 1. HERO SLIDER (CUSTOMIZABLE) -->
    <section class="relative h-[550px] md:h-[650px] overflow-hidden group">
      
      <!-- Slide Wrapper -->
      <div v-for="(slide, index) in heroSlides" :key="slide.id"
           class="absolute inset-0 transition-opacity duration-1000 ease-in-out"
           :class="index === currentSlide ? 'opacity-100 z-10' : 'opacity-0 z-0'">
        
        <!-- Background Image -->
        <div class="absolute inset-0 bg-cover bg-center transition-transform duration-[5000ms] ease-out"
             :style="{ backgroundImage: `url(${slide.image})` }"
             :class="index === currentSlide ? 'scale-105' : 'scale-100'">
        </div>
        
        <!-- Overlay -->
        <div class="absolute inset-0 bg-black/40"></div>

        <!-- Content -->
        <div class="absolute inset-0 flex items-center container mx-auto px-4"
             :class="{
               'justify-center text-center': slide.align === 'center',
               'justify-start text-left': slide.align === 'left',
               'justify-end text-right': slide.align === 'right'
             }">
          <div class="max-w-2xl text-white transform transition-all duration-700 delay-300"
               :class="index === currentSlide ? 'translate-y-0 opacity-100' : 'translate-y-10 opacity-0'">
            
            <p class="text-xs md:text-sm uppercase tracking-[0.4em] mb-4 text-gray-300">
              {{ slide.subtitle }}
            </p>
            <h1 class="text-5xl md:text-7xl font-bold mb-6 tracking-tighter leading-tight">
              {{ slide.title }}
            </h1>
            <p class="text-gray-200 text-sm md:text-base max-w-lg mb-8 font-light leading-relaxed"
               :class="slide.align === 'center' ? 'mx-auto' : 'ml-0'">
              {{ slide.desc }}
            </p>
            <button
              @click="router.push(slide.link)"
              class="bg-white text-black px-8 py-3 uppercase font-bold text-xs tracking-widest hover:bg-gray-200 hover:scale-105 transition-all duration-300 shadow-lg">
              {{ slide.btnText }}
            </button>
          </div>
        </div>
      </div>

      <!-- Controls: Prev/Next -->
      <button @click="prevSlide(); resetSlideTimer()" 
              class="absolute left-4 top-1/2 -translate-y-1/2 z-20 w-12 h-12 flex items-center justify-center border border-white/30 text-white hover:bg-white hover:text-black transition rounded-full opacity-0 group-hover:opacity-100 md:opacity-100">
        <i class="fa-solid fa-chevron-left"></i>
      </button>
      <button @click="nextSlide(); resetSlideTimer()" 
              class="absolute right-4 top-1/2 -translate-y-1/2 z-20 w-12 h-12 flex items-center justify-center border border-white/30 text-white hover:bg-white hover:text-black transition rounded-full opacity-0 group-hover:opacity-100 md:opacity-100">
        <i class="fa-solid fa-chevron-right"></i>
      </button>

      <!-- Indicators (Dots) -->
      <div class="absolute bottom-8 left-1/2 -translate-x-1/2 z-20 flex gap-3">
        <button v-for="(slide, index) in heroSlides" :key="index"
                @click="setSlide(index)"
                class="w-2.5 h-2.5 rounded-full transition-all duration-300"
                :class="index === currentSlide ? 'bg-white w-8' : 'bg-white/50 hover:bg-white'">
        </button>
      </div>
    </section>

    <!-- 2. DANH MỤC -->
    <section class="container mx-auto px-4 -mt-16 relative z-20">
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div v-for="(cat, index) in categories" :key="index"
          class="bg-white p-8 shadow-xl border border-gray-100 hover:shadow-2xl transition group cursor-pointer text-center transform hover:-translate-y-1">
          <div class="text-4xl mb-4 text-gray-800 group-hover:text-black transition duration-300">
            <i :class="cat.icon"></i>
          </div>
          <h3 class="text-xl font-bold uppercase tracking-wide mb-2">{{ cat.name }}</h3>
          <p class="text-gray-500 text-sm group-hover:text-gray-900 transition">Xem {{ cat.count }} sản phẩm</p>
        </div>
      </div>
    </section>

    <!-- 3. FLASH SALE -->
    <section class="container mx-auto px-4 mt-20 mb-10">
      <div class="border border-black p-6 flex flex-col md:flex-row items-center justify-between gap-6 bg-gray-50">
        <div class="flex items-center gap-4">
          <span class="bg-red-600 text-white px-3 py-1 text-xs font-bold uppercase blink-animation">GIỜ VÀNG</span>
          <h2 class="text-2xl font-bold uppercase tracking-tighter">Ưu Đãi Kết Thúc Trong</h2>
        </div>
        <div class="flex gap-4 font-mono text-2xl font-bold">
          <div class="flex flex-col items-center">
            <span class="bg-white border border-gray-200 w-12 h-12 flex items-center justify-center rounded shadow-sm">{{ timer.hours }}</span>
            <span class="text-xs font-light mt-1 text-gray-500">Giờ</span>
          </div>
          <span class="mt-2">:</span>
          <div class="flex flex-col items-center">
            <span class="bg-white border border-gray-200 w-12 h-12 flex items-center justify-center rounded shadow-sm">{{ timer.minutes }}</span>
            <span class="text-xs font-light mt-1 text-gray-500">Phút</span>
          </div>
          <span class="mt-2">:</span>
          <div class="flex flex-col items-center">
            <span class="bg-black text-white w-12 h-12 flex items-center justify-center rounded shadow-lg">{{ timer.seconds }}</span>
            <span class="text-xs font-light mt-1 text-gray-500">Giây</span>
          </div>
        </div>
        <button @click="router.push('/shop')" class="text-sm font-bold border-b-2 border-black hover:text-gray-600 hover:border-gray-600 transition">Xem tất cả deal ></button>
      </div>
    </section>

    <!-- 4. DANH SÁCH SẢN PHẨM -->
    <section class="container mx-auto px-4 py-10" id="shop-section">
      <div class="flex justify-between items-end mb-8">
        <div>
          <h2 class="text-3xl font-bold tracking-tighter uppercase">Sản Phẩm Mới</h2>
          <p class="text-gray-500 mt-2 text-sm">Cập nhật những xu hướng mới nhất</p>
        </div>
        <div class="h-1 w-20 bg-black"></div>
      </div>

      <!-- Wrapper Relative -->
      <div class="relative min-h-[400px]">

        <!-- Loading Overlay -->
        <div v-if="isLoading"
          class="absolute inset-0 z-10 bg-white/80 backdrop-blur-[2px] flex items-center justify-center transition-all duration-300">
          <div class="text-center">
            <i class="fa-solid fa-circle-notch fa-spin text-4xl text-black"></i>
          </div>
        </div>

        <!-- Product Grid -->
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 lg:grid-cols-5 gap-6">
          <div v-for="product in products" :key="product.id"
            class="group relative border border-gray-100 hover:border-black transition duration-300 bg-white hover:shadow-lg">
            
            <!-- Ảnh sản phẩm -->
            <div class="aspect-[3/4] bg-gray-100 relative overflow-hidden cursor-pointer" @click="goToDetail(product.id)">
              <span v-if="product.status == 1"
                class="absolute top-2 left-2 bg-black text-white text-[10px] px-2 py-1 uppercase font-bold z-10">Mới</span>

              <img :src="product.image || 'https://via.placeholder.com/300x400?text=No+Image'" alt="Product"
                class="w-full h-full object-cover object-center group-hover:scale-105 transition duration-700 ease-out"
                @error="$event.target.src = 'https://via.placeholder.com/300x400?text=Error'">

              <!-- Quick Action Buttons -->
              <div
                class="absolute bottom-4 left-0 right-0 flex justify-center gap-2 opacity-0 group-hover:opacity-100 transition duration-300 translate-y-4 group-hover:translate-y-0">
                <button
                  @click.stop="" 
                  class="bg-white text-black w-10 h-10 flex items-center justify-center shadow hover:bg-black hover:text-white transition rounded-full"
                  title="Thêm vào giỏ">
                  <i class="fa-solid fa-cart-plus"></i>
                </button>
                <button
                  @click.stop="goToDetail(product.id)"
                  class="bg-white text-black w-10 h-10 flex items-center justify-center shadow hover:bg-black hover:text-white transition rounded-full"
                  title="Xem chi tiết">
                  <i class="fa-regular fa-eye"></i>
                </button>
              </div>
            </div>

            <!-- Thông tin -->
            <div class="p-4 text-center">
              <h3 
                @click="goToDetail(product.id)"
                class="font-medium text-sm truncate uppercase tracking-wide cursor-pointer hover:underline mb-1">
                {{ product.name }}
              </h3>
              <p class="text-gray-500 text-xs line-clamp-1 mb-2">{{ product.description }}</p>
              <div class="font-bold text-lg">{{ formatCurrency(product.price) }}</div>
            </div>
          </div>
        </div>
      </div>

      <!-- 5. PHÂN TRANG -->
      <div class="flex justify-center mt-12 gap-2" v-if="totalPages > 1">
        <button @click="changePage(currentPage - 1)" :disabled="currentPage === 1 || isLoading"
          class="w-10 h-10 border border-gray-300 flex items-center justify-center hover:bg-black hover:text-white disabled:opacity-30 transition cursor-pointer disabled:cursor-not-allowed">
          <i class="fa-solid fa-chevron-left"></i>
        </button>

        <button v-for="page in totalPages" :key="page" @click="changePage(page)" :disabled="isLoading" :class="[
          'w-10 h-10 border flex items-center justify-center transition font-medium text-sm cursor-pointer',
          currentPage === page
            ? 'bg-black text-white border-black'
            : 'border-gray-300 hover:bg-gray-100'
        ]">
          {{ page }}
        </button>

        <button @click="changePage(currentPage + 1)" :disabled="currentPage === totalPages || isLoading"
          class="w-10 h-10 border border-gray-300 flex items-center justify-center hover:bg-black hover:text-white disabled:opacity-30 transition cursor-pointer disabled:cursor-not-allowed">
          <i class="fa-solid fa-chevron-right"></i>
        </button>
      </div>
    </section>

    <!-- 6. NEWS / TIN TỨC -->
    <section class="bg-gray-50 py-16 mt-10">
      <div class="container mx-auto px-4">
        <div class="text-center mb-12">
          <p class="text-gray-500 uppercase tracking-widest text-xs mb-2">Tin Tức Mới Nhất</p>
          <h2 class="text-3xl font-bold tracking-tighter uppercase">Tạp Chí Thời Trang</h2>
          <div class="h-1 w-10 bg-black mx-auto mt-4"></div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          <div v-for="news in latestNews" :key="news.id" class="group cursor-pointer">
            <div class="overflow-hidden mb-4 relative h-64">
               <div class="absolute top-4 left-4 bg-white px-3 py-1 text-xs font-bold shadow z-10">
                 {{ news.date }}
               </div>
              <img :src="news.image" alt="News" class="w-full h-full object-cover transform group-hover:scale-110 transition duration-700 ease-out">
            </div>
            <h3 class="text-xl font-bold mb-2 group-hover:text-gray-600 transition">{{ news.title }}</h3>
            <p class="text-gray-500 text-sm line-clamp-2 mb-4 leading-relaxed">{{ news.excerpt }}</p>
            <span class="text-xs font-bold uppercase border-b border-black pb-1 hover:text-gray-600 hover:border-gray-600 transition">Đọc thêm</span>
          </div>
        </div>
      </div>
    </section>

  </div>
</template>

<style scoped>
@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

.blink-animation {
  animation: blink 1.5s infinite;
}

/* Ẩn thanh cuộn nếu cần */
.no-scrollbar::-webkit-scrollbar {
  display: none;
}
.no-scrollbar {
  -ms-overflow-style: none;
  scrollbar-width: none;
}
</style>