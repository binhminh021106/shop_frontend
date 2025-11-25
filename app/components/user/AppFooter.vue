<template>
  <footer class="bg-black text-white pt-20 pb-10 mt-auto">
    <div class="container mx-auto px-6">
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-12 mb-16">

        <!-- Cột 1 -->
        <div class="space-y-6">
          <h2 class="text-3xl font-bold tracking-tighter">BW<span class="font-light text-gray-400">Store</span></h2>
          <p class="text-gray-400 text-sm leading-7 pr-4">
            Thương hiệu thời trang tối giản dành cho những người yêu thích sự tinh tế.
          </p>
          <div class="flex gap-4">
            <a href="#" class="social-icon"><i class="fa-brands fa-facebook-f"></i></a>
            <a href="#" class="social-icon"><i class="fa-brands fa-instagram"></i></a>
            <a href="#" class="social-icon"><i class="fa-brands fa-tiktok"></i></a>
          </div>
        </div>

        <!-- Cột 2 -->
        <div>
          <h3 class="footer-heading">Khám Phá</h3>
          <ul class="space-y-4 text-sm text-gray-400">
            <li>
              <NuxtLink to="/shop/men" class="footer-link">Thời Trang Nam</NuxtLink>
            </li>
            <li>
              <NuxtLink to="/shop/women" class="footer-link">Thời Trang Nữ</NuxtLink>
            </li>
            <li>
              <NuxtLink to="/shop/accessories" class="footer-link">Phụ Kiện</NuxtLink>
            </li>
          </ul>
        </div>

        <!-- Cột 3 -->
        <div>
          <h3 class="footer-heading">Hỗ Trợ</h3>
          <ul class="space-y-4 text-sm text-gray-400">
            <li>
              <NuxtLink to="/policy" class="footer-link">Chính sách đổi trả</NuxtLink>
            </li>
            <li>
              <NuxtLink to="/size-guide" class="footer-link">Hướng dẫn chọn size</NuxtLink>
            </li>
            <li>
              <NuxtLink to="/contact" class="footer-link">Liên hệ</NuxtLink>
            </li>
          </ul>
        </div>

        <!-- Cột 4 -->
        <div>
          <h3 class="footer-heading">Đăng Ký Tin</h3>
          <div class="space-y-4 text-sm text-gray-400">
            <p class="mb-2 text-white font-medium">Nhận ưu đãi mới nhất:</p>
            <form @submit.prevent class="flex">
              <input type="email" placeholder="Email..."
                class="bg-gray-900 text-white px-4 py-2 w-full outline-none focus:bg-gray-800 text-xs">
              <button class="bg-white text-black px-4 font-bold hover:bg-gray-200 uppercase text-xs">Gửi</button>
            </form>
          </div>
        </div>
      </div>

      <div
        class="border-t border-gray-800 pt-8 flex flex-col md:flex-row justify-between items-center text-xs text-gray-500">
        <p>&copy; 2024 BW Store. All rights reserved.</p>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.footer-heading {
  font-size: 1.125rem;
  font-weight: 700;
  margin-bottom: 1.5rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.footer-link {
  transition: all 0.3s;
  text-decoration: none;
  color: #9ca3af;
  /* gray-400 */
}

.footer-link:hover {
  color: white;
  padding-left: 0.5rem;
}

.social-icon {
  width: 2.5rem;
  height: 2.5rem;
  border: 1px solid #374151;
  /* gray-700 */
  border-radius: 9999px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s;
  color: white;
  text-decoration: none;
}

.social-icon:hover {
  background-color: white;
  color: black;
  border-color: white;
}
</style>