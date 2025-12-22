<template>
  <header class="h-16 bg-white border-b border-gray-200 flex items-center justify-between px-4 md:px-8 z-10 sticky top-0">
    <!-- Left: Mobile Toggle & Breadcrumb -->
    <div class="flex items-center gap-4">
      <button @click="toggleSidebar" class="md:hidden text-gray-500 hover:text-black focus:outline-none">
        <i class="fa-solid fa-bars text-xl"></i>
      </button>

      <nav class="hidden md:flex text-sm text-gray-500">
        <span>Admin</span>
        <span class="mx-2">/</span>
        <span class="font-semibold text-black">Dashboard</span>
      </nav>
    </div>

    <!-- Right: Actions -->
    <div class="flex items-center gap-4">
      <button class="relative p-2 text-gray-400 hover:text-black transition">
        <i class="fa-regular fa-bell"></i>
        <span class="absolute top-2 right-2 w-2 h-2 bg-red-500 rounded-full border border-white"></span>
      </button>
    </div>
  </header>
</template>

<script setup>
const isSidebarOpen = useState('sidebar-open')

const toggleSidebar = () => {
  isSidebarOpen.value = !isSidebarOpen.value
}
</script>