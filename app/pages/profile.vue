<script setup>
// 1. Layout & Meta
definePageMeta({
  layout: 'empty'
})

// --- STATE ---
const activeTab = ref('profile')
const isLoading = ref(false)
const fileInput = ref(null)

// Mock Data User
const userData = ref({
  fullName: 'NGUYEN VAN A',
  email: 'nguyenvana@example.com',
  phone: '0901234567',
  avatar: null,
  memberId: 'BW-88291',
  joinDate: '12/2023',
  addresses: [
    { 
      id: 1, 
      name: 'NGUYEN VAN A',
      phone: '0901234567',
      street: '123 Đường Số 1', 
      city: 'Quận 1, TP.HCM', 
      isDefault: true 
    },
    { 
      id: 2, 
      name: 'NGUYEN VAN B',
      phone: '0909999999',
      street: '456 Lê Văn Sỹ', 
      city: 'Quận 3, TP.HCM', 
      isDefault: false 
    }
  ]
})

// State form thêm địa chỉ
const isAddingAddress = ref(false)
const newAddress = ref({
  name: '',
  phone: '',
  street: '',
  city: '',
  isDefault: false
})

// Mock Data Orders
const orders = ref([
  { id: '#BW9928', date: '15/12/2025', total: '2.500.000₫', status: 'Đang giao', items: 2, image: 'https://assets.adidas.com/images/h_840,f_auto,q_auto,fl_lossy,c_fill,g_auto/c7c251c8cc904d9fb950a9d9e600a0b6_9366/Giay_Superstar_Trang_EG4958_01_standard.jpg' },
  { id: '#BW8812', date: '01/11/2025', total: '1.200.000₫', status: 'Hoàn thành', items: 1, image: 'https://assets.adidas.com/images/h_840,f_auto,q_auto,fl_lossy,c_fill,g_auto/6a0d6244621544608381af6e00f95b3b_9366/Giay_Ultraboost_Light_Xam_HQ6353_01_standard.jpg' }
])

const passwordForm = ref({ current: '', new: '', confirm: '' })

// --- ACTIONS ---

const switchTab = (tab) => {
  activeTab.value = tab
}

const triggerFileInput = () => {
  fileInput.value.click()
}

const handleFileChange = (event) => {
  const file = event.target.files[0]
  if (file) {
    userData.value.avatar = URL.createObjectURL(file)
  }
}

// Logic Address
const openAddAddress = () => {
  // Reset form
  newAddress.value = { name: userData.value.fullName, phone: userData.value.phone, street: '', city: '', isDefault: false }
  isAddingAddress.value = true
}

const saveNewAddress = () => {
  if (!newAddress.value.street || !newAddress.value.city) {
    alert('Vui lòng nhập địa chỉ và tỉnh/thành phố')
    return
  }

  // Nếu chọn mặc định, bỏ mặc định các cái cũ
  if (newAddress.value.isDefault || userData.value.addresses.length === 0) {
    userData.value.addresses.forEach(a => a.isDefault = false)
    newAddress.value.isDefault = true
  }

  userData.value.addresses.push({
    id: Date.now(),
    ...newAddress.value
  })

  isAddingAddress.value = false
}

const removeAddress = (id) => {
  const list = userData.value.addresses
  const item = list.find(x => x.id === id)
  if (item.isDefault && list.length > 1) {
    alert('Không thể xóa địa chỉ mặc định. Hãy chọn địa chỉ khác làm mặc định trước.')
    return
  }
  userData.value.addresses = list.filter(x => x.id !== id)
}

const setDefaultAddress = (id) => {
  userData.value.addresses.forEach(item => {
    item.isDefault = (item.id === id)
  })
}

const handleUpdateProfile = () => {
  isLoading.value = true
  setTimeout(() => {
    isLoading.value = false
    alert('Cập nhật thông tin thành công!')
  }, 800)
}

const handleLogout = () => {
  const confirm = window.confirm('Bạn có chắc muốn đăng xuất?')
  if(confirm) navigateTo('/login')
}

const getStatusColor = (status) => {
  if (status === 'Hoàn thành') return 'bg-green-100 text-green-800 border-green-200'
  if (status === 'Đang giao') return 'bg-yellow-100 text-yellow-800 border-yellow-200'
  return 'bg-gray-100 text-gray-800'
}
</script>

<template>
  <div class="h-screen flex font-inter text-black bg-white overflow-hidden relative">
    
    <!-- === POPUP ADD ADDRESS (MODAL) === -->
    <transition name="fade">
        <div v-if="isAddingAddress" class="fixed inset-0 z-50 flex items-center justify-center p-4">
            <!-- Backdrop -->
            <div class="absolute inset-0 bg-black/80 backdrop-blur-sm" @click="isAddingAddress = false"></div>
            
            <!-- Modal Content -->
            <div class="relative bg-white w-full max-w-lg p-8 shadow-2xl animate-fade-in-up z-10">
                <button @click="isAddingAddress = false" class="absolute top-4 right-4 text-gray-400 hover:text-black transition-colors">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
                
                <h3 class="text-xl font-black italic uppercase tracking-tighter mb-6 border-b border-gray-100 pb-4">Thêm địa chỉ mới</h3>
                
                <div class="space-y-4">
                    <div class="grid grid-cols-2 gap-4">
                        <div class="space-y-1">
                            <label class="text-[10px] font-bold uppercase tracking-widest text-gray-500">Họ tên</label>
                            <input v-model="newAddress.name" placeholder="Họ tên người nhận" class="w-full h-10 px-3 border border-gray-300 text-sm focus:border-black focus:outline-none focus:ring-1 focus:ring-black">
                        </div>
                        <div class="space-y-1">
                            <label class="text-[10px] font-bold uppercase tracking-widest text-gray-500">Số điện thoại</label>
                            <input v-model="newAddress.phone" placeholder="Số điện thoại" class="w-full h-10 px-3 border border-gray-300 text-sm focus:border-black focus:outline-none focus:ring-1 focus:ring-black">
                        </div>
                    </div>
                    
                    <div class="space-y-1">
                        <label class="text-[10px] font-bold uppercase tracking-widest text-gray-500">Địa chỉ</label>
                        <input v-model="newAddress.street" placeholder="Số nhà, tên đường..." class="w-full h-10 px-3 border border-gray-300 text-sm focus:border-black focus:outline-none focus:ring-1 focus:ring-black">
                    </div>
                    
                    <div class="space-y-1">
                         <label class="text-[10px] font-bold uppercase tracking-widest text-gray-500">Khu vực</label>
                        <input v-model="newAddress.city" placeholder="Tỉnh/Thành phố, Quận/Huyện" class="w-full h-10 px-3 border border-gray-300 text-sm focus:border-black focus:outline-none focus:ring-1 focus:ring-black">
                    </div>
                    
                    <div class="flex items-center pt-2">
                        <input id="default-addr" type="checkbox" v-model="newAddress.isDefault" class="w-4 h-4 text-black border-gray-300 rounded focus:ring-black accent-black cursor-pointer">
                        <label for="default-addr" class="ml-2 text-xs font-bold uppercase cursor-pointer select-none">Đặt làm địa chỉ mặc định</label>
                    </div>
                    
                    <div class="flex gap-3 pt-4">
                        <button type="button" @click="saveNewAddress" class="flex-1 h-12 bg-black text-white text-xs font-bold uppercase tracking-widest hover:bg-gray-800 transition-colors">Hoàn thành</button>
                        <button type="button" @click="isAddingAddress = false" class="flex-1 h-12 border border-gray-300 text-black text-xs font-bold uppercase tracking-widest hover:bg-gray-100 transition-colors">Hủy bỏ</button>
                    </div>
                </div>
            </div>
        </div>
    </transition>

    <!-- === SIDEBAR === -->
    <div class="hidden lg:flex w-3/12 xl:w-1/4 flex-shrink-0 bg-black flex-col justify-between p-8 relative z-10">
        <div class="absolute inset-0 opacity-20 bg-[url('https://www.transparenttextures.com/patterns/cubes.png')] pointer-events-none"></div>

        <div class="z-10">
            <div class="mb-8">
                <NuxtLink to="/" class="flex items-center gap-2 text-white hover:opacity-80 transition">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>
                    <span class="text-xs font-bold uppercase tracking-widest">Trang chủ</span>
                </NuxtLink>
            </div>
            
            <div class="border border-gray-800 p-6 mb-10 bg-white/5 backdrop-blur-sm">
                <div class="w-16 h-16 rounded-full flex items-center justify-center mb-4 mx-auto lg:mx-0 overflow-hidden bg-white border-2 border-white">
                    <img v-if="userData.avatar" :src="userData.avatar" class="w-full h-full object-cover">
                    <span v-else class="text-2xl font-black text-black">{{ userData.fullName.charAt(0) }}</span>
                </div>
                <h2 class="text-xl text-white font-black italic uppercase tracking-tighter">{{ userData.fullName }}</h2>
                <div class="mt-4 inline-block bg-white text-black text-[10px] font-bold px-2 py-1 uppercase tracking-widest">
                    MEMBER ID: {{ userData.memberId }}
                </div>
            </div>

            <nav class="space-y-1">
                <button @click="switchTab('profile')" :class="['w-full text-left py-4 px-2 border-b border-gray-800 flex justify-between group transition-colors duration-200', activeTab === 'profile' ? 'text-white border-white' : 'text-gray-500 hover:text-gray-300']">
                    <span class="text-sm font-bold uppercase tracking-widest">Thông tin tài khoản</span>
                    <svg v-if="activeTab === 'profile'" class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path></svg>
                </button>
                <button @click="switchTab('orders')" :class="['w-full text-left py-4 px-2 border-b border-gray-800 flex justify-between group transition-colors duration-200', activeTab === 'orders' ? 'text-white border-white' : 'text-gray-500 hover:text-gray-300']">
                    <span class="text-sm font-bold uppercase tracking-widest">Lịch sử đơn hàng</span>
                    <svg v-if="activeTab === 'orders'" class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path></svg>
                </button>
                <button @click="switchTab('security')" :class="['w-full text-left py-4 px-2 border-b border-gray-800 flex justify-between group transition-colors duration-200', activeTab === 'security' ? 'text-white border-white' : 'text-gray-500 hover:text-gray-300']">
                    <span class="text-sm font-bold uppercase tracking-widest">Bảo mật</span>
                    <svg v-if="activeTab === 'security'" class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path></svg>
                </button>
            </nav>
        </div>

        <div class="z-10 mt-auto">
            <button @click="handleLogout" class="flex items-center gap-3 text-gray-500 hover:text-red-500 transition-colors text-xs font-bold uppercase tracking-widest group">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" /></svg>
                <span class="group-hover:translate-x-1 transition-transform">Đăng xuất</span>
            </button>
        </div>
    </div>

    <!-- === MAIN CONTENT === -->
    <div class="w-full lg:w-9/12 xl:w-3/4 flex flex-col bg-white h-full overflow-y-auto relative custom-scrollbar">
        <!-- Mobile Toggle -->
        <div class="lg:hidden flex items-center justify-between p-6 border-b border-black sticky top-0 bg-white z-20">
             <div class="text-xl font-black italic tracking-tighter">BW<span class="font-light text-gray-400">STORE</span></div>
             <button class="p-2 border border-black"><svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path></svg></button>
        </div>

        <div class="p-6 lg:p-12 max-w-4xl mx-auto w-full min-h-[500px] pb-20">
            
            <!-- TAB: PROFILE -->
            <div v-if="activeTab === 'profile'">
                <div class="mb-8 pb-4 border-b border-gray-100 flex justify-between items-end">
                    <div>
                        <h1 class="text-3xl lg:text-4xl font-black uppercase italic tracking-tighter mb-2">Hồ sơ của tôi</h1>
                        <p class="text-xs text-gray-500 font-medium">Quản lý thông tin hồ sơ để bảo mật tài khoản</p>
                    </div>
                </div>

                <form @submit.prevent="handleUpdateProfile">
                    <!-- Avatar -->
                    <div class="mb-10 flex flex-col items-center sm:flex-row gap-6">
                        <div class="relative group cursor-pointer" @click="triggerFileInput">
                            <div class="w-24 h-24 rounded-full border-2 border-black overflow-hidden flex items-center justify-center bg-gray-100">
                                <img v-if="userData.avatar" :src="userData.avatar" class="w-full h-full object-cover">
                                <span v-else class="text-3xl font-black text-gray-400">{{ userData.fullName.charAt(0) }}</span>
                            </div>
                            <div class="absolute inset-0 rounded-full bg-black/50 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z" /><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 13a3 3 0 11-6 0 3 3 0 016 0z" /></svg>
                            </div>
                            <input type="file" ref="fileInput" class="hidden" accept="image/*" @change="handleFileChange">
                        </div>
                        <div class="text-center sm:text-left">
                            <button type="button" @click="triggerFileInput" class="text-xs font-bold uppercase underline hover:text-gray-600 mb-1">Thay đổi ảnh đại diện</button>
                            <p class="text-[10px] text-gray-400">Dung lượng file tối đa 1 MB. Định dạng: .JPEG, .PNG</p>
                        </div>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div class="md:col-span-2">
                            <label class="block text-[10px] font-bold uppercase tracking-widest mb-2 text-gray-500">Họ và tên</label>
                            <input v-model="userData.fullName" type="text" class="w-full h-12 px-4 border border-black text-sm font-bold focus:outline-none focus:ring-1 focus:ring-black rounded-none uppercase">
                        </div>
                        <div>
                            <label class="block text-[10px] font-bold uppercase tracking-widest mb-2 text-gray-500">Email</label>
                            <input v-model="userData.email" readonly type="email" class="w-full h-12 px-4 border border-gray-200 bg-gray-50 text-gray-500 text-sm font-bold focus:outline-none cursor-not-allowed rounded-none">
                        </div>
                         <div>
                            <label class="block text-[10px] font-bold uppercase tracking-widest mb-2 text-gray-500">Số điện thoại</label>
                            <input v-model="userData.phone" type="tel" class="w-full h-12 px-4 border border-black text-sm font-bold focus:outline-none focus:ring-1 focus:ring-black rounded-none">
                        </div>

                        <!-- ADDRESS LIST -->
                        <div class="md:col-span-2 mt-4">
                             <div class="flex justify-between items-center mb-4">
                                <label class="text-[10px] font-bold uppercase tracking-widest text-gray-500">Sổ địa chỉ</label>
                                <button type="button" @click="openAddAddress" class="text-[10px] font-bold uppercase underline hover:text-black flex items-center gap-1">
                                    <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4" /></svg>
                                    Thêm địa chỉ mới
                                </button>
                             </div>
                             
                             <div class="space-y-3">
                                 <div v-for="addr in userData.addresses" :key="addr.id" :class="['p-4 border flex justify-between items-start group transition-all', addr.isDefault ? 'border-black bg-gray-50' : 'border-gray-200']">
                                    <div>
                                        <div class="flex items-center gap-2 mb-1">
                                            <span class="text-sm font-bold">{{ addr.name }}</span>
                                            <span class="text-gray-400 text-xs font-normal">| {{ addr.phone }}</span>
                                        </div>
                                        <div class="text-xs text-gray-600">{{ addr.street }}</div>
                                        <div class="text-xs text-gray-600 mb-2">{{ addr.city }}</div>

                                        <div v-if="addr.isDefault" class="text-[10px] font-bold text-white bg-black px-2 py-0.5 inline-block uppercase tracking-wider">Mặc định</div>
                                        <button v-else type="button" @click="setDefaultAddress(addr.id)" class="text-[10px] font-bold text-gray-400 hover:text-black underline uppercase tracking-wider">Đặt làm mặc định</button>
                                    </div>
                                    <button type="button" @click="removeAddress(addr.id)" class="text-gray-300 hover:text-red-500 transition-colors p-1">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" /></svg>
                                    </button>
                                 </div>
                             </div>
                        </div>

                        <div class="md:col-span-2 mt-8 pt-6 border-t border-gray-100">
                            <button type="submit" :disabled="isLoading" class="group h-12 bg-black text-white flex items-center px-8 hover:bg-gray-800 transition-all duration-300 disabled:opacity-70">
                                <span v-if="!isLoading" class="text-xs font-bold uppercase tracking-widest mr-4">LƯU THAY ĐỔI</span>
                                <span v-else class="text-xs font-bold uppercase tracking-widest mr-4">ĐANG XỬ LÝ...</span>
                                <i class="fa-solid fa-arrow-right transform group-hover:translate-x-2 transition-transform"></i>
                            </button>
                        </div>
                    </div>
                </form>
            </div>

            <!-- TAB: ORDERS -->
            <div v-else-if="activeTab === 'orders'">
                <div class="mb-8 pb-4 border-b border-gray-100">
                    <h1 class="text-3xl lg:text-4xl font-black uppercase italic tracking-tighter mb-2">Đơn hàng của bạn</h1>
                    <p class="text-xs text-gray-500 font-medium">{{ orders.length }} đơn hàng gần đây</p>
                </div>
                <div class="space-y-4">
                    <div v-for="order in orders" :key="order.id" class="border border-gray-200 hover:border-black transition-colors p-4 md:p-6 flex flex-col md:flex-row gap-6 items-start md:items-center group cursor-pointer bg-white">
                        <div class="w-20 h-20 bg-gray-100 flex-shrink-0 relative overflow-hidden">
                            <img :src="order.image" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500 mix-blend-multiply">
                        </div>
                        <div class="flex-1">
                            <div class="flex justify-between items-start mb-2">
                                <h3 class="text-lg font-bold uppercase">{{ order.id }}</h3>
                                <span :class="['text-[10px] px-2 py-1 uppercase font-bold border', getStatusColor(order.status)]">{{ order.status }}</span>
                            </div>
                            <div class="text-xs text-gray-500 mb-1">Ngày đặt: {{ order.date }}</div>
                            <div class="text-xs text-gray-500">Số lượng: {{ order.items }} sản phẩm</div>
                        </div>
                        <div class="flex flex-row md:flex-col items-center md:items-end gap-4 md:gap-1 w-full md:w-auto justify-between border-t md:border-t-0 border-gray-100 pt-4 md:pt-0">
                            <span class="text-sm font-bold">{{ order.total }}</span>
                            <span class="text-[10px] font-bold uppercase border-b border-black cursor-pointer hover:text-gray-600">Xem chi tiết</span>
                        </div>
                    </div>
                </div>
            </div>

            <!-- TAB: SECURITY -->
            <div v-else-if="activeTab === 'security'">
                <div class="mb-8 pb-4 border-b border-gray-100">
                    <h1 class="text-3xl lg:text-4xl font-black uppercase italic tracking-tighter mb-2">Đổi mật khẩu</h1>
                    <p class="text-xs text-gray-500 font-medium">Để bảo mật tài khoản, vui lòng không chia sẻ mật khẩu cho người khác</p>
                </div>
                <div class="max-w-md">
                    <div class="space-y-6">
                        <div>
                            <label class="block text-[10px] font-bold uppercase tracking-widest mb-2 text-gray-500">Mật khẩu hiện tại</label>
                            <input v-model="passwordForm.current" type="password" class="w-full h-12 px-4 border border-black text-sm font-bold focus:outline-none focus:ring-1 focus:ring-black rounded-none">
                        </div>
                        <div>
                            <label class="block text-[10px] font-bold uppercase tracking-widest mb-2 text-gray-500">Mật khẩu mới</label>
                            <input v-model="passwordForm.new" type="password" class="w-full h-12 px-4 border border-black text-sm font-bold focus:outline-none focus:ring-1 focus:ring-black rounded-none">
                        </div>
                        <div>
                            <label class="block text-[10px] font-bold uppercase tracking-widest mb-2 text-gray-500">Xác nhận mật khẩu mới</label>
                            <input v-model="passwordForm.confirm" type="password" class="w-full h-12 px-4 border border-black text-sm font-bold focus:outline-none focus:ring-1 focus:ring-black rounded-none">
                        </div>
                        <button class="w-full h-12 bg-black text-white text-xs font-bold uppercase tracking-widest hover:bg-gray-800 transition-colors">Cập nhật mật khẩu</button>
                    </div>
                </div>
            </div>

        </div>
    </div>
  </div>
</template>

<style scoped>
/* Chỉ giữ lại animation fade-in cho các phần tử nhỏ, bỏ transition cho tab container */
.animate-fade-in {
  animation: fadeIn 0.3s ease-out forwards;
}

.animate-fade-in-up {
  animation: fadeInUp 0.4s ease-out forwards;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

/* Scrollbar Styling */
.custom-scrollbar::-webkit-scrollbar {
  width: 6px;
}
.custom-scrollbar::-webkit-scrollbar-track {
  background: transparent;
}
.custom-scrollbar::-webkit-scrollbar-thumb {
  background-color: rgba(0,0,0,0.1);
  border-radius: 3px;
}
.custom-scrollbar:hover::-webkit-scrollbar-thumb {
  background-color: rgba(0,0,0,0.3);
}

/* Vue Transition for Modal */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>