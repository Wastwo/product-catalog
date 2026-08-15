<script setup>
defineProps({
  filteredProducts: {
    type: Array,
    default: () => [],
  },
  isFiltering: {
    type: Boolean,
    default: false,
  },
})

const searchQuery = defineModel('searchQuery', { type: String, default: '' })
const selectedCategory = defineModel('selectedCategory', { type: String, default: 'Semua' })
const sortByPrice = defineModel('sortByPrice', { type: String, default: 'default' })
</script>

<template>
  <div
    class="bg-white p-5 font-sans rounded-2xl border border-card-border/80 shadow-sm space-y-4 transition-all duration-300 hover:shadow-md"
  >
    <div class="flex flex-col md:flex-row gap-3 items-center">
      <div class="relative w-full flex-1 group">
        <input
          v-model="searchQuery"
          type="text"
          placeholder="Cari produk di Tokopedia..."
          class="w-full pl-10 pr-10 py-2.5 bg-page-bg-alt border border-card-border rounded-xl text-sm font-sans text-text-primary placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-brand-primary/20 focus:border-brand-primary focus:bg-card-bg transition-all duration-200"
        />
        <span
          class="absolute left-3.5 top-3 text-text-muted group-focus-within:text-brand-primary transition-colors"
        >
          <svg
            class="w-4 h-4"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
            />
          </svg>
        </span>

        <button
          v-if="searchQuery"
          @click="searchQuery = ''"
          class="absolute right-3 top-2.5 p-1 text-text-muted hover:text-text-primary rounded-full hover:bg-card-border/60 transition-colors"
          aria-label="Clear search"
        >
          <svg
            class="w-3.5 h-3.5"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2.5"
          >
            <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>

      <div class="relative w-full md:w-52">
        <select
          v-model="selectedCategory"
          class="w-full pl-3.5 pr-8 py-2.5 bg-page-bg-alt border border-card-border rounded-xl text-sm font-medium text-text-primary focus:outline-none focus:ring-2 focus:ring-brand-primary/20 focus:border-brand-primary focus:bg-white transition-all cursor-pointer appearance-none"
        >
          <option value="Semua">Semua Kategori</option>
          <option value="Fashion">Fashion</option>
          <option value="Elektronik">Elektronik</option>
          <option value="Aksesoris">Aksesoris</option>
        </select>
        <div
          class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-3 text-text-muted"
        >
          <svg
            class="w-4 h-4"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
          </svg>
        </div>
      </div>

      <div class="relative w-full md:w-52">
        <select
          v-model="sortByPrice"
          class="w-full pl-3.5 pr-8 py-2.5 bg-page-bg-alt border border-card-border rounded-xl text-sm font-medium text-text-primary focus:outline-none focus:ring-2 focus:ring-brand-primary/20 focus:border-brand-primary focus:bg-white transition-all cursor-pointer appearance-none"
        >
          <option value="default">Urutan Default</option>
          <option value="asc">Harga: Termurah</option>
          <option value="desc">Harga: Termahal</option>
        </select>
        <div
          class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-3 text-text-muted"
        >
          <svg
            class="w-4 h-4"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
          </svg>
        </div>
      </div>
    </div>

    <div class="flex items-center justify-between text-xs text-text-muted font-sans">
      <span
        >Menampilkan
        <strong class="text-text-primary font-bold">{{ filteredProducts.length }}</strong>
        produk</span
      >
      <span
        v-if="isFiltering"
        class="text-brand-primary font-semibold inline-flex items-center gap-1.5"
      >
        <svg class="w-3.5 h-3.5 animate-spin text-brand-primary" fill="none" viewBox="0 0 24 24">
          <circle
            class="opacity-25"
            cx="12"
            cy="12"
            r="10"
            stroke="currentColor"
            stroke-width="4"
          ></circle>
          <path
            class="opacity-75"
            fill="currentColor"
            d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
          ></path>
        </svg>
        Menyaring hasil...
      </span>
    </div>
  </div>
</template>
