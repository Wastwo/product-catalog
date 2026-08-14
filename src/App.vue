<script setup>
import { ref, onMounted } from 'vue'

const masterProduk = ref([])
const isLoading = ref(true)
const errorMessage = ref('')

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
    gambar: '<https://images.unsplash.com/photo-1602810318383-e386cc2a3ccf?w=300&q=80>',
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
  } catch (error) {
    errorMessage.value = error.message
  } finally {
    isLoading.value = false
  }
}

onMounted(() => {
  loadData()
})
</script>

<template>
  <div></div>
</template>
