<script setup>
import { ref, onMounted, watch } from 'vue'
import ProductList from './components/katalog/ProductList.vue'
import ProductSearch from './components/katalog/ProductSearch.vue'
import ProductDetailModal from './components/katalog/ProductDetailModal.vue'
import ProductFormModal from './components/katalog/ProductFormModal.vue'

const masterProduk = ref([])
const isLoading = ref(true)
const errorMessage = ref('')
const searchQuery = ref('')
const selectedCategory = ref('Semua')
const sortByPrice = ref('default')
const filteredProducts = ref([])
const isFiltering = ref(false)
let debounceTimer = null

const selectedProduct = ref(null)
const isDetailOpen = ref(false)

const isAddModalOpen = ref(false)

const openDetailModal = (product) => {
  selectedProduct.value = product
  isDetailOpen.value = true
}

const closeDetailModal = () => {
  isDetailOpen.value = false
  selectedProduct.value = null
}

const handleAddProduct = (newProduct) => {
  masterProduk.value.unshift(newProduct)
  applyFilterAndSort()
}

const dummyData = [
  {
    id: 1,
    nama: 'Kemeja Flannel Slimfit Tartan Green',
    harga: 185000,
    kategori: 'Fashion',
    stok: 12,
    rating: 4.8,
    terjual: 140,
    lokasi: 'Kota Bandung',
    gambar: 'https://images.unsplash.com/photo-1602810318383-e386cc2a3ccf?w=300&q=80',
    deskripsi: 'Bahan Cotton Combed 30s adem dan menyerap keringat.',
  },
  {
    id: 2,
    nama: 'T-Shirt Green Tokopedia',
    harga: 85000,
    kategori: 'Fashion',
    stok: 25,
    rating: 4.8,
    terjual: 120,
    lokasi: 'Jakarta Selatan',
    gambar: 'https://placehold.co/300x300?text=T-Shirt',
    deskripsi: 'Kaos katun combed 30s adem dan nyaman digunakan sehari-hari.',
  },
]

const loadKatalogProduk = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const isError = false

      if (isError) {
        reject(new Error('Gagal memuat data dari server. Silakan coba lagi.'))
      } else {
        resolve(dummyData)
      }
    }, 1500)
  })
}

const loadData = async () => {
  isLoading.value = true
  errorMessage.value = ''
  try {
    const response = await loadKatalogProduk()
    masterProduk.value = response
    applyFilterAndSort()
  } catch (error) {
    errorMessage.value = error.message
  } finally {
    isLoading.value = false
  }
}

const applyFilterAndSort = () => {
  let masterProdukCloned = [...masterProduk.value]

  if (searchQuery.value.trim() !== '') {
    const cleanedQuery = searchQuery.value.trim().toLowerCase()
    masterProdukCloned = masterProdukCloned.filter(
      (item) =>
        item.nama.toLowerCase().includes(cleanedQuery) ||
        item.kategori.toLowerCase().includes(cleanedQuery),
    )
  }

  if (selectedCategory.value !== 'Semua') {
    masterProdukCloned = masterProdukCloned.filter(
      (item) => item.kategori === selectedCategory.value,
    )
  }

  if (sortByPrice.value === 'asc') {
    masterProdukCloned = masterProdukCloned.sort((a, b) => a.harga - b.harga)
  } else if (sortByPrice.value === 'desc') {
    masterProdukCloned = masterProdukCloned.sort((a, b) => b.harga - a.harga)
  }

  filteredProducts.value = masterProdukCloned
}

const resetFilters = () => {
  searchQuery.value = ''
  selectedCategory.value = 'Semua'
  sortByPrice.value = 'default'
}

watch([searchQuery, selectedCategory, sortByPrice], () => {
  if (debounceTimer) clearTimeout(debounceTimer)
  isFiltering.value = true

  debounceTimer = setTimeout(() => {
    applyFilterAndSort()
    isFiltering.value = false
  }, 350)
})

onMounted(() => {
  loadData()
})
</script>

<template>
  <div class="min-h-screen bg-page-bg p-4 md:p-8">
    <div class="max-w-6xl mx-auto space-y-6">
      <header
        class="flex items-center justify-between bg-white p-4 rounded-xl border border-card-border shadow-sm"
      >
        <div class="flex items-center gap-3">
          <div
            class="w-9 h-9 bg-brand-primary rounded-lg flex items-center justify-center text-white font-extrabold font-display text-xl"
          >
            T
          </div>
          <div class="flex flex-col gap-1 justify-center">
            <h1
              class="text-xl font-bold font-display text-text-primary hover:text-brand-primary transition-colors"
            >
              Katalog Produk
            </h1>
            <p class="text-xs font-sans text-text-muted">Inspirasi Belanja Harian Kamu</p>
          </div>
        </div>
        <div class="flex justify-center items-center gap-2">
          <button
            @click="loadData"
            class="flex justify-center items-center gap-2 px-4 py-2 bg-brand-primary hover:bg-emerald-600 rounded-lg active:scale-95 transition-colors cursor-pointer"
          >
            <svg class="w-4 h-4 text-white" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 80 80">
              <path
                fill="currentColor"
                d="M7.207,71.5l5.659-5.659l-0.328-0.354C6.064,58.534,2.5,49.482,2.5,40C2.5,19.322,19.322,2.5,40,2.5 c1.696,0,3.499,0.146,5.5,0.448v9.098C43.659,11.684,41.812,11.5,40,11.5c-15.715,0-28.5,12.785-28.5,28.5 c0,7.031,2.628,13.803,7.399,19.066l0.353,0.389l6.248-6.249V71.5H7.207z"
              ></path>
              <path
                fill="currentColor"
                d="M40,3c1.55,0,3.193,0.125,5,0.38v8.064C43.325,11.149,41.649,11,40,11c-15.991,0-29,13.009-29,29 c0,7.156,2.674,14.047,7.529,19.402l0.705,0.778l0.743-0.743L25,54.414V71H8.414l4.464-4.464l0.682-0.682l-0.657-0.706 C6.517,58.285,3,49.355,3,40C3,19.598,19.598,3,40,3 M40,2C19.013,2,2,19.013,2,40c0,9.987,3.867,19.055,10.172,25.828L6,72h20V52 l-6.73,6.73C14.773,53.771,12,47.224,12,40c0-15.464,12.536-28,28-28c2.062,0,4.065,0.238,6,0.661V2.519 C44.041,2.207,42.047,2,40,2L40,2z"
              ></path>
              <g>
                <path
                  fill="currentColor"
                  d="M40,77.5c-1.696,0-3.499-0.146-5.5-0.448v-9.098c1.841,0.362,3.687,0.546,5.5,0.546 c15.715,0,28.5-12.785,28.5-28.5c0-7.043-2.625-13.817-7.391-19.075l-0.353-0.389L54.5,26.793V8.5h18.293l-5.676,5.675l0.328,0.354 C73.93,21.495,77.5,30.541,77.5,40C77.5,60.678,60.678,77.5,40,77.5z"
                ></path>
                <path
                  fill="currentColor"
                  d="M71.586,9l-4.481,4.481l-0.682,0.682l0.657,0.706C73.477,21.743,77,30.668,77,40 c0,20.402-16.598,37-37,37c-1.55,0-3.193-0.125-5-0.38v-8.064C36.675,68.851,38.351,69,40,69c15.991,0,29-13.009,29-29 c0-7.168-2.671-14.061-7.52-19.411l-0.705-0.778l-0.743,0.743L55,25.586V9H71.586 M74,8H54v20l6.739-6.739 C65.235,26.221,68,32.776,68,40c0,15.464-12.536,28-28,28c-2.062,0-4.065-0.238-6-0.661v10.142C35.959,77.793,37.953,78,40,78 c20.987,0,38-17.013,38-38c0-9.987-3.884-19.038-10.188-25.812L74,8L74,8z"
                ></path>
              </g>
            </svg>
            <span class="font-sans text-xs font-semibold text-white">Refresh Data</span>
          </button>
          <button
            @click="isAddModalOpen = true"
            class="flex justify-center items-center gap-2 px-4 py-2 bg-brand-primary hover:bg-emerald-600 rounded-lg active:scale-95 transition-colors cursor-pointer"
          >
            <svg
              class="w-4 h-4 text-white"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              stroke-width="2.5"
            >
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m7.5-7.5h-15" />
            </svg>
            <span class="font-sans text-xs font-semibold text-white">Tambah Produk</span>
          </button>
        </div>
      </header>

      <ProductSearch
        :filtered-products="filteredProducts"
        :is-filtering="isFiltering"
        v-model:search-query="searchQuery"
        v-model:sort-by-price="sortByPrice"
        v-model:selected-category="selectedCategory"
      />

      <ProductList
        :filtered-products="filteredProducts"
        :is-loading="isLoading"
        :error-message="errorMessage"
        @retry="loadData"
        @reset-filter="resetFilters"
        @select-product="openDetailModal"
      />

      <ProductDetailModal
        :is-open="isDetailOpen"
        :produk="selectedProduct"
        @close="closeDetailModal"
      />

      <ProductFormModal
        :is-open="isAddModalOpen"
        @close="isAddModalOpen = false"
        @add-product="handleAddProduct"
      />
    </div>
  </div>
</template>
