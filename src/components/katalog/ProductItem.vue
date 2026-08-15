<script setup>
import BaseCard from '../base/BaseCard.vue'

defineProps({
  produk: {
    type: Object,
    required: true,
  },
})

function formatCurrency(nominal) {
  return new Intl.NumberFormat('id-ID', {
    style: 'currency',
    currency: 'IDR',
    maximumFractionDigits: 0,
  }).format(nominal)
}
</script>

<template>
  <BaseCard>
    <template #media>
      <img :src="produk.gambar" :alt="produk.nama" class="w-full h-40 object-cover bg-page-bg" />
      <span
        class="absolute top-2 left-2 bg-brand-primary text-white text-[10px] font-sans font-bold px-2 py-0.5 rounded-full shadow-sm"
      >
        Official Store
      </span>
    </template>

    <div>
      <h3
        class="text-xs md:text-sm font-display font-semibold text-text-primary line-clamp-2 leading-snug"
      >
        {{ produk.nama }}
      </h3>

      <p class="font-sans text-brand-primary font-extrabold text-sm md:text-base mt-1">
        {{ formatCurrency(produk.harga) }}
      </p>

      <div
        class="font-sans text-[11px] text-text-muted mt-1 inline-flex justify-center items-center gap-1"
      >
        <svg
          class="w-3.5 h-3.5 shrink-0"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z" />
          <circle cx="12" cy="10" r="3" />
        </svg>
        <span class="inline-block leading-none">{{ produk.lokasi }}</span>
      </div>
    </div>

    <template #footer>
      <div class="flex items-center justify-between font-sans text-[11px]">
        <div
          class="text-secondary-accent font-sans font-bold inline-flex justify-center items-center gap-1"
        >
          <svg class="w-3.5 h-3.5 fill-current shrink-0" viewBox="0 0 24 24">
            <path
              d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"
            />
          </svg>
          <span class="inline-block leading-none">{{ produk.rating }}</span>
        </div>

        <span class="text-text-muted"> Terjual {{ produk.terjual }} </span>
      </div>
    </template>
  </BaseCard>
</template>
