<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import {
  NavigationMenu,
} from '@/components/ui/navigation-menu'
import { useColorMode } from '@vueuse/core'
import { Icon } from '@iconify/vue'
import { Button } from '@/components/ui/button'
import { Toggle } from '@/components/ui/toggle'
import { cartItems } from '@/store/cartState'
import { computed } from 'vue'
import Footer from './components/Footer.vue'
import { ref } from 'vue'

const isMobileMenuOpen = ref(false) // Track menu state

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value // Toggle menu
}

const cartItemCount = computed(() => cartItems.value.length)

const navitems = [
  {
    icon: '',
    href: '/',
    label: 'Home'
  },
  {
    icon: '',
    href: '/about',
    label: 'About'
  },
  {
    icon: '',
    href: '/policy',
    label: 'Policy'
  }
  // Add more items as needed
]

const colorMode = useColorMode()

const toggleColorMode = () => {
  colorMode.value = colorMode.value === 'dark' ? 'light' : 'dark'
}
</script>

<template>
  <header class="sticky top-0 z-10 bg-card py-2">
    <NavigationMenu class="flex items-center justify-between mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
      <RouterLink to="/" class="flex items-center gap-2">
        <img src="../src/assets/logo.png" class="h-8 w-8 transition-all" alt="V Care Logo" />
        <span class="font-semibold text-lg">V Care</span>
      </RouterLink>

      <div class="hidden md:flex items-center gap-6">
        <RouterLink
          v-for="item in navitems"
          :key="item.href"
          :to="item.href"
          class="flex items-center gap-2 text-md font-medium transition-colors hover:text-pink-500 focus:text-pink-500"
        >
          <Icon :icon="item.icon" class="h-4 w-4 transition-all" />
          {{ item.label }}
        </RouterLink>
      </div>

      <div class="flex items-center gap-4">
        <RouterLink to="/cart" class="relative flex items-center p-2">
          <Icon
            icon="mdi:cart-outline"
            class="h-6 w-6 transition-all text-gray-700 dark:text-white"
          />
          <span
            v-if="cartItemCount"
            class="absolute -top-1 -right-2 bg-red-500 text-white text-xs rounded-full px-2"
          >
            {{ cartItemCount }}
          </span>
        </RouterLink>

        <button
          @click="toggleColorMode"
          class="p-2 rounded-full bg-gray-200 dark:bg-gray-800 transition"
        >
          <Icon icon="radix-icons:moon" class="h-5 w-5 dark:hidden" />
          <Icon icon="radix-icons:sun" class="h-5 w-5 hidden dark:block" />
        </button>

        <!-- Mobile Menu Button -->
        <button @click="toggleMobileMenu" class="md:hidden p-2 rounded-lg">
          <Icon icon="mdi:menu" class="h-6 w-6 text-gray-700 dark:text-white" />
        </button>
      </div>
    </NavigationMenu>

    <!-- Mobile Menu -->
    <div v-if="isMobileMenuOpen" class="md:hidden bg-card py-2 px-4">
      <RouterLink
        v-for="item in navitems"
        :key="item.href"
        :to="item.href"
        class="block py-2 text-gray-700 dark:text-white transition-colors hover:text-pink-500"
      >
        {{ item.label }}
      </RouterLink>
    </div>
  </header>

  <main class="min-h-screen py-10">
    <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
      <RouterView />
    </div>
  </main>

  <Footer />
</template>
