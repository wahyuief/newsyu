<template>
    <div class="mb-10">
        <UHorizontalNavigation :links="links" class="border-b border-gray-200 dark:border-gray-800" />
    </div>
</template>

<script setup lang="ts">
const route = useRoute()

const links = [
  [{
    label: 'Home',
    icon: 'i-heroicons-home',
    to: '/'
  }, {
    label: 'About',
    icon: 'i-heroicons-user',
    to: '/about'
  }, {
    label: 'Contact',
    icon: 'i-heroicons-chat-bubble-left-right',
  }], 
  [{
    label: 'Login',
    icon: 'i-heroicons-arrow-right-end-on-rectangle'
  }]
]
</script>