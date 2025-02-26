<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { RouterLink } from 'vue-router'
import { Skeleton } from '@/components/ui/skeleton'
import { Icon } from '@iconify/vue/dist/iconify.js'
import { Card, CardContent } from '@/components/ui/card'
import { addToCart as sharedAddToCart } from '@/store/cartState'

type Product = {
  id: number
  title: string
  price: number
  description: string
  category: string
  thumbnail: string
}

type ApiResponse = {
  products: Product[]
  total: number
  skip: number
  limit: number
}

const products = ref<Product[]>([])
const cart = ref<Product[]>([]) // Cart state
const loading = ref(false)
const isInCart = ref(false)
const fetchProducts = async () => {
  loading.value = true
  const limit = 20
  const skip = 0
  try {
    const response = await fetch(`https://dummyjson.com/products?limit=${limit}&skip=${skip}`)
    const data: ApiResponse = await response.json()
    products.value = data.products
  } catch (error) {
    console.error('Error fetching products:', error)
  } finally {
    loading.value = false
  }
}

const addToCart = (product: Product) => {
  // Check if product is already in cart
  sharedAddToCart({
    id: product.id,
    title: product.title,
    price: product.price,
    thumbnail: product.thumbnail,
    quantity: 1
  })
}

onMounted(() => {
  fetchProducts()
})
</script>

<template>
  
  <!-- Product List -->
  <h2 class="text-2xl mt-8 font-semibold text-foreground">Products List</h2>

  <!-- Product Loading -->
  <div
    v-if="loading"
    class="mt-8 grid grid-cols-1 gap-y-12 sm:grid-cols-2 sm:gap-x-6 lg:grid-cols-4 xl:gap-x-8"
  >
    <div class="flex flex-col space-y-3" v-for="n in 8" :key="n">
      <Skeleton class="h-[288px] w-[280px] bg-gray-200 dark:bg-gray-800 rounded-xl" />
      <div class="space-y-2">
        <Skeleton class="h-4 w-[250px] bg-gray-200 dark:bg-gray-800" />
        <Skeleton class="h-4 w-[200px] bg-gray-200 dark:bg-gray-800" />
      </div>
      <Skeleton class="h-10 w-full space-y-10 rounded-md bg-gray-200 dark:bg-gray-800" />
    </div>
  </div>

  <!-- Product Card -->
  <div
    v-else
    class="my-8 grid grid-cols-1 gap-y-12 sm:grid-cols-2 sm:gap-x-6 lg:grid-cols-4 xl:gap-x-8"
  >
    <div v-for="product in products" :key="product.id">
      <Card class="relative max-w-sm rounded-3xl p-4">

      <CardContent class="space-y-4 border-none">
        <div>
          <RouterLink :to="'/product/' + product.id" class="block">
          <!-- Image -->
        <div class="aspect-square overflow-hidden rounded-2xl">
          <img
            :src="product.thumbnail"
            :alt="product.title"
            class="h-56 w-full object-cover object-center transition-transform duration-300 hover:scale-110"
          />
        </div>

        <!-- Product Info -->
        <div class="space-y-2">
          <h3 class="text-xl h-14 line-clamp-2 font-semibold">
            {{ product.title }}
          </h3>
          <p class="text-[15px] font-medium text-gray-500 dark:text-gray-400 line-clamp-2">
            {{ product.description }}
          </p>
          <div class="space-y-1">
            <p class="text-[15px] font-medium text-gray-500 dark:text-gray-400">
              Shop: <span class="text-blue-600 dark:text-blue-400">T-Care</span>
            </p>
            <p class="text-[15px] font-medium text-gray-500 dark:text-gray-400">
              Category: {{ product.category }}
            </p>
          </div>
        </div>
      </RouterLink>
        </div>
    

        <!-- Price and Favorite -->
        <div class="flex items-center justify-between">
          <p class="text-2xl font-bold text-pink-500">
            ${{ product.price }}
          </p>
          <button 
            class="rounded-full p-2 hover:text-pink-500 transition-colors"
            @click="addToCart(product)"
          >
            <Icon 
              :icon="isInCart ? 'mdi:cart-check' : 'mdi:cart-plus'" 
              class="h-6 w-6"
            />
          </button>
        </div>
      </CardContent>
  </Card>
      <!-- <div class="mt-6 flex justify-between gap-3">
        <RouterLink :to="'/product/' + product.id">
          <Button
            class="bg-yellow-500 hover:bg-yellow-600 text-gray-50 hover:text-gray-100 rounded-full px-4 flex items-center gap-2"
          >
            <Icon icon="lucide:shopping-bag" class="w-4 h-4" />
            Product Detail<span class="sr-only">, {{ product.title }}</span>
          </Button>
        </RouterLink>
        <Button
          class="text-gray-50 hover:text-gray-100 flex rounded-full px-4 items-center gap-2"
          @click="addToCart(product)"
        >
          <Icon icon="ion:cart-outline" class="w-6 h-6" />
          Add to cart<span class="sr-only">, {{ product.title }}</span>
        </Button>
      </div> -->
    </div>
  </div>
</template>
