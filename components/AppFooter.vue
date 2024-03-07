<template>
    <div class="border-t border-gray-200 dark:border-gray-800 mt-10 mb-10">
        <div class="grid grid-cols-4 gap-4 pt-4">
          <div class="col-span-2">
            <h3 class="text-lg font-semibold">NewsYu</h3>
            <p class="text-gray-400">Portal berita terdepan, menyajikan informasi terkini dan teraktual. Konsisten memberikan informasi fakta sejak 2010 hingga kini tanpa lelah.</p>
          </div>
          <div>
            <h3 class="text-lg font-semibold">Categories</h3>
            <ul class="text-gray-400">
              <li><a href="#">Artis</a></li>
              <li><a href="#">Bisnis</a></li>
              <li><a href="#">Gadget</a></li>
              <li><a href="#">Olahraga</a></li>
            </ul>
          </div>
          <div>
            <h3 class="text-lg font-semibold">Links</h3>
            <ul class="text-gray-400">
              <li><nuxt-link to="/about">About Us</nuxt-link></li>
              <li><a href="#">Contact Us</a></li>
              <li><a href="#">Privacy Policy</a></li>
              <li><a href="#">Terms</a></li>
            </ul>
          </div>
        </div>
    </div>
</template>