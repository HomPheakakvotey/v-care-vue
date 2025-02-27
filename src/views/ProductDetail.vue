<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import axios from 'axios'
import { useRoute } from 'vue-router'
import { Skeleton } from '@/components/ui/skeleton'
import { cartItems, addToCart as sharedAddToCart } from '@/store/cartState' 
import ProductList from '@/components/ProductList.vue'

type Product = {
  id: number
  name: string
  price: number
  description: string
  category: string
  rating: number
  thumbnail: string
}

const product = ref<Product | null>(null)
const loading = ref(false)
const route = useRoute()

const fetchProductDetail = async (productId: number) => {
  loading.value = true
  try {
    const response = await axios.get(`https://api-vcare.lyhou.engineer/api/products/${productId}`)
    product.value = response.data
  } catch (error) {
    console.error('Error fetching product detail:', error)
  } finally {
    loading.value = false
  }
}

const addToCart = (product: Product | null) => {
  if (!product) return

  // Check if product is already in cart
  const existingItem = cartItems.value.find((cartItem) => cartItem.id === product.id)
  if (!existingItem) {
    sharedAddToCart({
      id: product.id,
      title: product.name,
      price: product.price,
      thumbnail: product.thumbnail,
      quantity: 1
    })
  } else {
    console.log(`Product with id ${product.id} already exists in the cart`)
  }
}

// Fetch the product when the component is mounted
onMounted(() => {
  const productId = Number(route.params.id)
  if (productId) {
    fetchProductDetail(productId)
  }
})

// Watch for changes in the route's id and refetch the product details
watch(() => route.params.id, (newId) => {
  const productId = Number(newId)
  if (productId) {
    fetchProductDetail(productId)
  }
})
</script>

<template>
  <div v-if="loading">
    <div
      class="mx-auto max-w-2xl px-4 py-16 sm:px-6 sm:py-24 lg:grid lg:max-w-7xl lg:grid-cols-2 lg:gap-x-8 lg:px-8"
    >
      <!-- Skeleton for Product name -->
      <div class="lg:max-w-lg lg:self-end mt-4">
        <Skeleton class="h-12 w-full bg-gray-200 dark:bg-gray-800 rounded-lg" />
      </div>

      <!-- Skeleton for Product Price and Description -->
      <div class="lg:max-w-lg lg:self-end mt-4">
        <Skeleton class="h-8 w-1/4 bg-gray-200 dark:bg-gray-800 rounded-lg mb-4" />
        <Skeleton class="h-32 w-full bg-gray-200 dark:bg-gray-800 rounded-lg" />
      </div>

      <!-- Skeleton for Product Image -->
      <div class="mt-10 lg:col-start-2 lg:row-span-2 lg:mt-0 lg:self-center">
        <div
          class="aspect-h-1 aspect-w-1 overflow-hidden rounded-lg border border-gray-200 dark:border-gray-800"
        >
          <Skeleton class="h-full w-full bg-gray-200 dark:bg-gray-700" />
        </div>
      </div>

      <!-- Skeleton for Add to Bag Button -->
      <div class="mt-10 lg:col-start-1 lg:row-start-2 lg:max-w-lg lg:self-start">
        <Skeleton class="h-14 w-full bg-gray-200 dark:bg-gray-800 rounded-lg" />
      </div>
    </div>
  </div>

  <div v-else class="bg-card">
    <div class="mx-auto grid grid-cols-1 md:grid-cols-2 gap-16 items-center p-6 ">
  <!-- Product Image -->
  <div v-if="product" class="flex justify-center ">
    <div class="w-full max-w-sm overflow-hidden rounded-lg">
      <img
        :src="product.thumbnail"
        :alt="product.name"
        class="h-80 w-full object-cover rounded-lg shadow-md"
      />
    </div>
  </div>

  <!-- Product Details -->
  <div v-if="product" class="space-y-6">

    <h1 class="text-3xl font-bold tracking-tight sm:text-4xl">
      {{ product.name }}
    </h1>

      <div class="flex items-center">
        <p class="text-3xl font-bold text-pink-500">${{ product.price }}</p>
      </div>

      <div class="my-2">
        <p class="text-gray-700 dark:text-gray-400 text-lg">{{ product.description }}</p>
      </div>

      <div class="flex items-center">
        <p class="text-gray-700 dark:text-gray-400 text-lg">Shop: <span class="text-blue-600 dark:text-blue-600">T-Care</span></p>
      </div>

      <div class="flex items-center">
        <p class="text-gray-700 dark:text-gray-400 text-lg"> Category: {{ product.category }}</p>
      </div>

    <!-- Add to Cart Button -->
    <div class="mt-6">
      <button
        @click="addToCart(product)"
        class="w-full bg-pink-500 hover:bg-pink-600 text-white font-semibold py-3 rounded-lg transition-all"
      >
        Add to Cart
      </button>
    </div>
  </div>
</div>

  </div>

  <ProductList />
</template>
