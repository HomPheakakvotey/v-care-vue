<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import { ref } from 'vue'
import { Icon } from '@iconify/vue'

const email = ref('')

const handleEmailSignup = () => {
  console.log('Email signup:', email.value)
  // Implement your email signup logic here
  email.value = '' // Clear the input after submission
}

const socials = [
  {
    name: 'Twitter',
    icon: 'mdi:twitter',
    url: '#'
  },
  {
    name: 'Facebook',
    icon: 'mdi:facebook',
    url: '#'
  },
  {
    name: 'LinkedIn',
    icon: 'mdi:linkedin',
    url: '#'
  }
]

const discoverLinks = [
  { name: 'Trending Today', url: '#' },
  { name: 'Shop all', url: '#' },
  { name: '% Sales', url: '#' }
]

const aboutLinks = [
  { name: 'Help Center', url: '#' },
  { name: 'Shipping/Returns', url: '#' },
  { name: 'Affiliates', url: '#' }
]

const legalLinks = [
  { name: 'Contact', url: '#' },
  { name: 'Privacy Policies', url: '#' },
  { name: 'Terms & Conditions', url: '#' }
]
</script>




<template>
    <footer class="py-12 bg-card">
      <div class="container mx-auto px-4">
        <!-- Top section with logo, email signup, and social -->
        <div class="mb-12 grid gap-8 lg:grid-cols-2">
          <!-- Logo and signup section -->
          <div class="space-y-6">
            <div class="flex items-center space-x-2">
              <img src="../assets/logo.svg" alt="Logo" class="h-8 w-8" />
            </div>
            <div class="max-w-md">
              <p class="mb-4 text-xl text-gray-600 dark:text-gray-400">
                Sign up for offers and more
              </p>
              <form @submit.prevent="handleEmailSignup" class="flex gap-2">
                <input
                  v-model="email"
                  type="email"
                  placeholder="Enter your email here"
                  class="flex-1 rounded-md border border-gray-300 px-4 py-2   dark:border-gray-700 dark:bg-gray-800 dark:text-white"
                  required
                />
                <button
                  type="submit"
                  class="rounded-md bg-pink-500 px-4 py-2 text-white transition-colors hover:bg-pink-600"
                >
                  <Icon icon="material-symbols:arrow-forward" class="h-5 w-5" />
                </button>
              </form>
              <!-- Social Icons -->
              <div class="mt-6 flex space-x-4">
                <a
                  v-for="social in socials"
                  :key="social.name"
                  :href="social.url"
                  class="text-gray-500 transition-colors hover:text-gray-900 dark:text-gray-400 dark:hover:text-white"
                  :aria-label="social.name"
                >
                  <Icon :icon="social.icon" class="h-5 w-5" />
                </a>
              </div>
            </div>
          </div>
  
          <!-- Footer Links -->
          <div class="grid gap-8 sm:grid-cols-3">
            <!-- Discover Column -->
            <div>
              <h3 class="mb-4 font-semibold text-xl text-gray-900 dark:text-white">Discover</h3>
              <ul class="space-y-3">
                <li v-for="link in discoverLinks" :key="link.name">
                  <a
                    :href="link.url"
                    class="text-lg text-gray-600 transition-colors hover:text-gray-900 dark:text-gray-400 dark:hover:text-white"
                  >
                    {{ link.name }}
                  </a>
                </li>
              </ul>
            </div>
  
            <!-- About Column -->
            <div>
              <h3 class="mb-4 font-semibold text-xl text-gray-900 dark:text-white">About</h3>
              <ul class="space-y-3">
                <li v-for="link in aboutLinks" :key="link.name">
                  <a
                    :href="link.url"
                    class="text-lg text-gray-600 transition-colors hover:text-gray-900 dark:text-gray-400 dark:hover:text-white"
                  >
                    {{ link.name }}
                  </a>
                </li>
              </ul>
            </div>
  
            <!-- Legal Column -->
            <div>
              <h3 class="mb-4 font-semibold text-xl text-gray-900 dark:text-white">Legal</h3>
              <ul class="space-y-3">
                <li v-for="link in legalLinks" :key="link.name">
                  <a
                    :href="link.url"
                    class="text-lg text-gray-600 transition-colors hover:text-gray-900 dark:text-gray-400 dark:hover:text-white"
                  >
                    {{ link.name }}
                  </a>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </footer>
  </template>
  
