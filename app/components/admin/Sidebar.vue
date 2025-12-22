<template>
  <!-- === SIDEBAR (Desktop) === -->
  <aside class="hidden md:flex flex-col w-72 bg-white border-r border-gray-100 shadow-[4px_0_24px_rgba(0,0,0,0.02)] z-30 h-screen sticky top-0 font-sans">
    
    <!-- 1. LOGO AREA -->
    <div class="h-20 flex items-center px-8 border-b border-gray-50 shrink-0">
      <NuxtLink to="/admin" class="flex items-center gap-3 text-2xl font-extrabold tracking-tight text-slate-800 no-underline hover:opacity-80 transition">
        <div class="w-10 h-10 bg-black text-white rounded-xl flex items-center justify-center shadow-lg shadow-gray-200">
          <i class="fa-solid fa-cube text-lg"></i>
        </div>
        <span>BW<span class="text-gray-400 font-medium">System</span></span>
      </NuxtLink>
    </div>

    <!-- 2. NAVIGATION (Scrollable) -->
    <nav class="flex-1 px-4 py-6 overflow-y-auto custom-scrollbar flex flex-col gap-6">
      
      <!-- Loop qua từng nhóm menu (Dashboard, Quản lý, Hệ thống...) -->
      <div v-for="(group, groupIndex) in menuGroups" :key="groupIndex">
        
        <!-- Tên nhóm (Header nhỏ) -->
        <p v-if="group.label" class="px-4 mb-3 text-[11px] font-bold text-gray-400 uppercase tracking-widest font-mono">
          {{ group.label }}
        </p>

        <div class="flex flex-col space-y-1">
          <!-- Loop qua từng item trong nhóm -->
          <div v-for="(item, itemIndex) in group.items" :key="itemIndex">
            
            <!-- TRƯỜNG HỢP 1: Item đơn (Không có dropdown) -->
            <NuxtLink v-if="!item.children" :to="item.to"
              class="flex items-center px-4 py-3 text-[15px] font-medium rounded-xl transition-all duration-200 group relative"
              active-class="bg-black text-white shadow-md shadow-gray-200"
              :class="!isActive(item.to) ? 'text-gray-600 hover:bg-gray-50 hover:text-black' : ''"
            >
              <i :class="[item.icon, 'w-6 text-center mr-3 transition-colors', isActive(item.to) ? 'text-white' : 'text-gray-400 group-hover:text-black']"></i>
              <span>{{ item.label }}</span>
            </NuxtLink>

            <!-- TRƯỜNG HỢP 2: Item có Dropdown (Có con) -->
            <div v-else>
              <!-- Nút cha để mở dropdown -->
              <button @click="toggleMenu(item.key)"
                class="w-full flex items-center justify-between px-4 py-3 text-[15px] font-medium rounded-xl transition-colors duration-200 hover:bg-gray-50 text-gray-600 hover:text-black group"
                :class="{'bg-gray-50 text-black': openMenus[item.key] || isChildActive(item.children)}"
              >
                <div class="flex items-center">
                  <i :class="[item.icon, 'w-6 text-center mr-3 text-gray-400 group-hover:text-black transition-colors', (openMenus[item.key] || isChildActive(item.children)) ? '!text-black' : '']"></i>
                  <span>{{ item.label }}</span>
                </div>
                <!-- Mũi tên xoay -->
                <i class="fa-solid fa-chevron-down text-xs text-gray-400 transition-transform duration-300"
                  :class="{'rotate-180': openMenus[item.key]}"></i>
              </button>

              <!-- Danh sách con (Dropdown body) -->
              <div v-show="openMenus[item.key]" class="mt-1 ml-4 pl-4 border-l border-gray-100 space-y-1 overflow-hidden transition-all">
                <NuxtLink v-for="(child, childIndex) in item.children" :key="childIndex"
                  :to="child.to"
                  class="flex items-center px-4 py-2.5 text-sm font-medium rounded-lg transition-colors relative"
                  active-class="text-blue-600 bg-blue-50/50"
                  :class="!isActive(child.to) ? 'text-gray-500 hover:text-black hover:bg-gray-50' : ''"
                >
                  <!-- Dấu chấm tròn nhỏ trang trí -->
                  <span class="w-1.5 h-1.5 rounded-full mr-3 transition-colors" 
                    :class="isActive(child.to) ? 'bg-blue-600' : 'bg-gray-300'"></span>
                  {{ child.label }}
                </NuxtLink>
              </div>
            </div>

          </div>
        </div>
      </div>

    </nav>

    <!-- 3. USER FOOTER -->
    <div class="p-5 border-t border-gray-100 shrink-0 bg-white">
      <button class="flex items-center gap-3 w-full p-3 rounded-2xl hover:bg-gray-50 border border-transparent hover:border-gray-200 transition-all group text-left">
        <img src="https://ui-avatars.com/api/?name=Admin+User&background=random" alt="Admin"
          class="w-10 h-10 rounded-full shadow-sm ring-2 ring-white">
        <div class="flex-1 min-w-0">
          <p class="text-sm font-bold text-gray-900 truncate">Admin User</p>
          <p class="text-xs text-gray-500 truncate">Quản trị viên</p>
        </div>
        <i class="fa-solid fa-arrow-right-from-bracket text-gray-400 group-hover:text-red-500 transition-colors"></i>
      </button>
    </div>
  </aside>

  <!-- === MOBILE OVERLAY (Giữ nguyên logic logic, cập nhật style) === -->
  <div v-if="isSidebarOpen" class="fixed inset-0 z-50 flex md:hidden">
    <div @click="isSidebarOpen = false" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm transition-opacity"></div>
    <div class="relative flex-1 flex flex-col max-w-xs w-full bg-white h-full shadow-2xl animate-slide-in">
        <!-- Nội dung mobile tương tự desktop, bạn có thể tái sử dụng component hoặc copy logic v-for ở trên xuống đây -->
        <div class="p-6">
            <h2 class="text-xl font-bold">Menu</h2>
            <!-- Mobile Menu content here -->
        </div>
    </div>
  </div>
</template>

<script setup>
const route = useRoute();
const isSidebarOpen = useState('sidebar-open');

// Quản lý trạng thái đóng mở của các dropdown menu
// Sử dụng reactive để Vue theo dõi sự thay đổi
const openMenus = reactive({
  products: true, // Mặc định mở menu sản phẩm
  sales: false,
  system: false
});

const toggleMenu = (key) => {
  openMenus[key] = !openMenus[key];
};

// Helper check active route
const isActive = (path) => route.path === path;

// Helper check nếu con đang active thì cha cũng sáng lên
const isChildActive = (children) => {
  if (!children) return false;
  return children.some(child => route.path === child.to);
};

// === CẤU TRÚC DATA MENU (Đã tinh gọn) ===
const menuGroups = [
  {
    items: [
      { label: 'Tổng quan', to: '/admin', icon: 'fa-solid fa-chart-pie' }
    ]
  },
  {
    label: 'Quản lý cửa hàng',
    items: [
      {
        label: 'Sản phẩm',
        icon: 'fa-solid fa-box-archive',
        key: 'products', // Key để toggle
        children: [
          // Đã bỏ mục "Thêm mới", người dùng sẽ thêm mới từ trang Danh sách
          { label: 'Tất cả sản phẩm', to: '/admin/products' },
          { label: 'Danh mục', to: '/admin/categories' },
          { label: 'Thuộc tính', to: '/admin/attributes' },
        ]
      },
      {
        label: 'Kho hàng',
        icon: 'fa-solid fa-warehouse',
        key: 'inventory',
        children: [
           { label: 'Nhập kho', to: '/admin/inventory/in' },
           { label: 'Kiểm kê', to: '/admin/inventory/check' },
        ]
      }
    ]
  },
  {
    label: 'Bán hàng',
    items: [
      {
        label: 'Đơn hàng',
        icon: 'fa-solid fa-clipboard-list',
        key: 'orders',
        children: [
          { label: 'Danh sách đơn hàng', to: '/admin/orders' },
          { label: 'Vận chuyển', to: '/admin/shipping' },
          { label: 'Hoàn trả / Khiếu nại', to: '/admin/returns' },
        ]
      },
      { label: 'Khách hàng', to: '/admin/customers', icon: 'fa-solid fa-users' },
    ]
  },
  {
    label: 'Hệ thống',
    items: [
      { label: 'Cấu hình chung', to: '/admin/settings', icon: 'fa-solid fa-gear' },
      { label: 'Phân quyền & Tài khoản', to: '/admin/users', icon: 'fa-solid fa-shield-halved' },
    ]
  }
];
</script>

<style scoped>
@keyframes slideIn {
  from { transform: translateX(-100%); }
  to { transform: translateX(0); }
}
.animate-slide-in {
  animation: slideIn 0.3s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

/* Custom Scrollbar tinh tế hơn */
.custom-scrollbar::-webkit-scrollbar {
  width: 5px;
}
.custom-scrollbar::-webkit-scrollbar-track {
  background: transparent;
}
.custom-scrollbar::-webkit-scrollbar-thumb {
  background: #e2e8f0;
  border-radius: 10px;
}
.custom-scrollbar::-webkit-scrollbar-thumb:hover {
  background: #cbd5e1;
}
</style>