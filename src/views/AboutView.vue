<script setup lang="ts">
import { ref } from 'vue'

const images = ref([
  { src: 'public/office.jpg?height=300&width=400', alt: 'Our office building' },
  { src: 'public/team.jpg?height=300&width=400', alt: 'Team collaboration' },
  { src: 'public/dior.jpg?height=300&width=400', alt: 'Product showcase' },
  { src: 'public/lab.jpg?height=300&width=400', alt: 'Client meeting' },
  { src: 'public/client.jpg?height=300&width=400', alt: 'Innovation lab' },
  { src: 'public/event.jpg?height=300&width=400', alt: 'Community event' },
])
</script>


<template>
  <div class="min-h-screen bg-card rounded-lg p-8">
    <main class="container mx-auto">
      <h1 class="text-4xl font-bold mb-8 text-center text-primary">About Us</h1>
      
      <section class="space-y-5">
        <h2 class="text-2xl font-semibold ">Our Story</h2>
        <p class="text-gray-700 dark:text-gray-400 text-lg">
          Welcome to V-Care, where beauty meets innovation. Founded in 2025, we are a passionate team of beauty enthusiasts and skincare experts on a mission to revolutionize how people experience self-care.Every product we offer is thoughtfully curated to cater to various skin types, tones, and preferences, ensuring that everyone can find something to enhance their natural beauty.
        </p>
        <p class="text-gray-700 dark:text-gray-400 text-lg">
          Our mission is to bring cutting-edge, high-quality beauty solutions to the forefront, transforming how people approach skincare and makeup. We are committed to delivering products that not only solve real-world beauty concerns but also promote confidence, innovation, and customer satisfaction.
        </p>
      </section>

      <section class="space-y-5">
        <h2 class="text-2xl font-semibold mt-5">Our Work</h2>
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
          <div v-for="(image, index) in images" :key="index" class="overflow-hidden rounded-lg">
            <img :src="image.src" :alt="image.alt" class="w-full h-64 object-cover transition-transform duration-300 hover:scale-105">
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

