<template>
  <header class="border-b border-gray-200 sticky top-0 bg-white/95 backdrop-blur z-50">
    <div class="container mx-auto px-6 h-20 flex items-center justify-between">

      <!-- 1. LOGO -->
      <div class="w-1/6">
        <!-- Trong Nuxt bạn có thể đổi NuxtLink thành NuxtLink -->
        <NuxtLink to="/" class="text-2xl font-bold tracking-tighter uppercase no-underline text-black">
          BW<span class="font-light">Store</span>
        </NuxtLink>
      </div>

      <!-- 2. MENU + SEARCH (Desktop) -->
      <div class="hidden md:flex flex-1 justify-center items-center gap-8">
        <nav class="flex gap-8 text-sm font-medium uppercase tracking-wide">
          <NuxtLink to="/" class="nav-link">Trang Chủ</NuxtLink>
          <NuxtLink to="/shop" class="nav-link">Cửa Hàng</NuxtLink>
          <NuxtLink to="/collection" class="nav-link text-gray-400 hover:text-black">Bộ Sưu Tập</NuxtLink>
        </nav>

        <div class="h-4 w-px bg-gray-300"></div>

        <div class="relative group">
          <input type="text" placeholder="Tìm kiếm..."
            class="pl-3 pr-8 py-1.5 border-b border-gray-300 focus:border-black outline-none bg-transparent text-sm w-48 transition-all focus:w-64 placeholder-gray-400">
          <button class="absolute right-0 top-1/2 -translate-y-1/2 text-gray-400 group-hover:text-black">
            <i class="fa-solid fa-magnifying-glass"></i>
          </button>
        </div>
      </div>

      <!-- 3. USER ACTIONS -->
      <div class="w-1/6 flex justify-end items-center gap-5">

        <NuxtLink to="/wishlist" class="relative group transform transition hover:scale-110">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
            stroke="currentColor" class="w-6 h-6 text-gray-700 group-hover:text-black transition-colors">
            <path stroke-linecap="round" stroke-linejoin="round"
              d="M21 8.25c0-2.485-2.099-4.5-4.688-4.5-1.935 0-3.597 1.126-4.312 2.733-.715-1.607-2.377-2.733-4.313-2.733C5.1 3.75 3 5.765 3 8.25c0 7.22 9 12 9 12s9-4.78 9-12z" />
          </svg>
          <span
            class="absolute -top-1.5 -right-1.5 inline-flex items-center justify-center w-4 h-4 text-[10px] font-bold text-white bg-black rounded-full border border-white">
            3
          </span>
        </NuxtLink>

        <NuxtLink to="/cart" class="relative group transform transition hover:scale-110">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
            stroke="currentColor" class="w-6 h-6 text-gray-700 group-hover:text-black transition-colors">
            <path stroke-linecap="round" stroke-linejoin="round"
              d="M15.75 10.5V6a3.75 3.75 0 10-7.5 0v4.5m11.356-1.993l1.263 12c.07.665-.45 1.243-1.119 1.243H4.25a1.125 1.125 0 01-1.12-1.243l1.264-12A1.125 1.125 0 015.513 7.5h12.974c.576 0 1.059.435 1.119 1.007z" />
          </svg>
          <span
            class="absolute -top-1.5 -right-1.5 inline-flex items-center justify-center w-4 h-4 text-[10px] font-bold text-white bg-black rounded-full border border-white">
            3
          </span>
        </NuxtLink>

        <NuxtLink to="/login" class="group transform transition hover:scale-110">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
            stroke="currentColor" class="w-6 h-6 text-gray-700 group-hover:text-black transition-colors">
            <path stroke-linecap="round" stroke-linejoin="round"
              d="M15.75 6a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0zM4.501 20.118a7.5 7.5 0 0114.998 0A17.933 17.933 0 0112 21.75c-2.676 0-5.216-.584-7.499-1.632z" />
          </svg>
        </NuxtLink>

        <button @click="toggleMenu" class="md:hidden ml-2 text-black hover:text-gray-600 transition">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
            stroke="currentColor" class="w-7 h-7">
            <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5" />
          </svg>
        </button>

      </div>
    </div>

    <!-- Mobile Menu Dropdown -->
    <transition name="slide-fade">
      <div v-if="isMenuOpen" class="md:hidden border-t border-gray-100 p-6 bg-white absolute w-full shadow-lg z-40">
        <nav class="flex flex-col gap-4 text-center">
          <NuxtLink to="/" class="font-medium hover:bg-gray-50 py-2 text-black no-underline" @click="toggleMenu">Trang
            Chủ</NuxtLink>
          <NuxtLink to="/shop" class="font-medium hover:bg-gray-50 py-2 text-black no-underline" @click="toggleMenu">Cửa
            Hàng</NuxtLink>
          <NuxtLink to="/cart" class="font-medium hover:bg-gray-50 py-2 text-black no-underline" @click="toggleMenu">Giỏ
            Hàng (2)</NuxtLink>
          <div class="relative w-full mt-2">
            <input type="text" placeholder="Tìm kiếm sản phẩm..."
              class="w-full border border-gray-300 p-2 rounded text-sm outline-none">
            <i class="fa-solid fa-magnifying-glass absolute right-3 top-3 text-gray-400"></i>
          </div>
        </nav>
      </div>
    </transition>
  </header>
</template>

<script setup>
import { ref } from 'vue';

const isMenuOpen = ref(false);
const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};
</script>

<style scoped>
.nav-link {
  position: relative;
  text-decoration: none;
  color: black;
}

.nav-link::after {
  content: '';
  position: absolute;
  width: 0;
  height: 1px;
  bottom: -2px;
  left: 0;
  background-color: black;
  transition: width 0.3s ease;
}

.nav-link:hover::after {
  width: 100%;
}

/* Transitions cho Mobile Menu */
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.2s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(-10px);
  opacity: 0;
}
</style>