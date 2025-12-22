<template>
  <div class="flex min-h-screen bg-gray-50 font-inter text-gray-800">
    <!-- Sidebar Component -->
    <AdminSidebar />

    <!-- Main Content Wrapper -->
    <div class="flex-1 flex flex-col min-w-0 overflow-hidden relative">
      <!-- Header Component -->
      <AdminHeader />

      <!-- Main Page Content -->
      <main class="flex-1 overflow-y-auto p-4 md:p-8 scroll-smooth">
        <div class="max-w-7xl mx-auto min-h-[calc(100vh-10rem)]">
          <slot />
        </div>
      </main>

      <!-- Footer Component -->
      <AdminFooter />
    </div>
  </div>
</template>

<script setup>
const isSidebarOpen = useState('sidebar-open', () => false)
</script>