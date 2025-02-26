<script setup lang="ts">
import { ref } from 'vue'
import ProductList from '@/components/ProductList.vue'
import Autoplay from 'embla-carousel-autoplay'
import { Carousel, CarouselContent, CarouselItem } from '@/components/ui/carousel'

const loading = ref(false)
// eslint-disable-next-line @typescript-eslint/no-unused-vars
const banner = ref([
  { id: 1, title: 'Product 1', image: 'public/image.png' },
  { id: 2, title: 'Product 2', image: 'public/dior.jpg' },
  { id: 3, title: 'Product 3', image: 'public/romand.png' }
])

const plugin = Autoplay({
  delay: 2000,
  stopOnMouseEnter: true,
  stopOnInteraction: false
})
</script>

<template>
  <main>
    <Skeleton v-if="loading" class="h-96 w-full bg-gray-200 dark:bg-gray-800 rounded-lg" />
    <!-- Hero Carousel -->
    <Carousel
      class="relative"
      :plugins="[
        Autoplay({
          delay: 2000
        })
      ]"
      :opts="{
        loop: true
      }"
      @mouseenter="plugin.stop"
      @mouseleave="[plugin.reset(), plugin.play()]"
    >
      <CarouselContent>
        <CarouselItem v-for="banner in banner" :key="banner.id">
          <div>
            <Card class="w-full">
              <CardContent class="flex h-full items-center justify-center">
                <img
                  :src="banner.image"
                  alt="Product Image"
                  class="object-cover w-full h-96 rounded-lg"
                />
              </CardContent>
            </Card>
          </div>
        </CarouselItem>
      </CarouselContent>
    </Carousel>
    <ProductList />
  </main>
</template>
