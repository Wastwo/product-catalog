<script setup>
defineProps({
  masterProduk: {
    type: Array,
    default: () => [],
  },
  isLoading: {
    type: Boolean,
    default: true,
  },
  errorMessage: {
    type: String,
    default: '',
  },
})

const emit = defineEmits(['retry'])
</script>

<template>
  <div v-if="isLoading" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4">
    <div
      v-for="n in 4"
      :key="n"
      class="bg-white rounded-xl border border-card-border p-3 animate-pulse space-y-3"
    >
      <div class="w-full h-40 bg-card-border rounded-lg"></div>
      <div class="h-4 bg-card-border rounded w-5/6"></div>
      <div class="h-5 bg-card-border rounded w-1/2"></div>
      <div class="h-3 bg-card-border rounded w-2/3"></div>
    </div>
  </div>

  <div
    v-else-if="errorMessage"
    class="flex flex-col gap-4 justify-center items-center bg-rose-50 border border-rose-200 text-rose-700 p-6 rounded-xl text-center my-8"
  >
    <div class="flex justify-center items-center gap-2 font-semibold font-sans text-lg mb-2">
      <svg
        class="w-5 h-5"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      >
        <path d="m21.73 18-8-14a2 2 0 0 0-3.48 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3Z" />
        <line x1="12" y1="9" x2="12" y2="13" />
        <line x1="12" y1="17" x2="12.01" y2="17" />
      </svg>
      <span>{{ errorMessage }}</span>
    </div>
    <button
      @click="emit('retry')"
      class="px-4 py-2 bg-rose-600 text-white rounded-lg hover:bg-rose-700 text-sm font-sans font-semibold transition-colors shadow-sm cursor-pointer active:scale-95"
    >
      Coba Lagi
    </button>
  </div>

  <div
    v-else-if="masterProduk.length === 0"
    class="flex flex-col justify-center items-center gap-4 bg-white rounded-xl border border-card-border p-12 text-center my-8"
  >
    <svg
      class="w-8 h-8"
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      stroke-width="2"
      stroke-linecap="round"
      stroke-linejoin="round"
    >
      <path
        d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"
      ></path>
      <polyline points="3.27 6.96 12 12.01 20.73 6.96"></polyline>
      <line x1="12" y1="22.08" x2="12" y2="12"></line>
    </svg>
    <div class="flex flex-col justify-center items-center gap-1">
      <h3 class="font-bold font-sans text-text-primary text-lg">Produk Tidak Ditemukan</h3>
      <p class="text-text-muted font-sans text-sm">
        Saat ini belum ada produk yang tersedia di katalog.
      </p>
    </div>
  </div>

  <div v-else class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4"></div>
</template>
