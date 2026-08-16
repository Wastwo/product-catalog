<script setup>
import BaseModal from '../base/BaseModal.vue'

defineProps({
  isOpen: {
    type: Boolean,
    default: false,
  },
  produk: {
    type: Object,
    default: null,
  },
})

defineEmits(['close'])

function formatCurrency(nominal) {
  if (!nominal) return 'Rp 0'
  return new Intl.NumberFormat('id-ID', {
    style: 'currency',
    currency: 'IDR',
    maximumFractionDigits: 0,
  }).format(nominal)
}
</script>

<template>
  <BaseModal :is-open="isOpen" :title="produk?.nama || 'Detail Produk'" @close="$emit('close')">
    <div v-if="produk" class="space-y-4 font-sans">
      <img
        :src="produk.gambar"
        :alt="produk.nama"
        class="w-full h-56 object-cover rounded-xl border border-card-border"
      />

      <div class="flex items-center justify-between">
        <span
          class="px-2.5 py-1 bg-brand-primary/10 text-brand-primary font-bold text-xs rounded-lg"
        >
          {{ produk.kategori }}
        </span>
        <span class="text-xs text-text-muted font-medium">
          Stok Tersedia: <strong>{{ produk.stok }}</strong>
        </span>
      </div>

      <div>
        <h4 class="text-xs font-bold text-text-muted uppercase tracking-wider mb-1">Deskripsi</h4>
        <p
          class="text-sm text-text-primary leading-relaxed bg-page-bg p-3 rounded-xl border border-card-border/60"
        >
          {{ produk.deskripsi || 'Tidak ada deskripsi produk.' }}
        </p>
      </div>

      <div class="flex items-center justify-between pt-2">
        <span class="text-xs text-text-muted">Harga Satuan</span>
        <span class="text-lg font-extrabold text-brand-primary">
          {{ formatCurrency(produk.harga) }}
        </span>
      </div>
    </div>

    <template #footer>
      <button
        @click="$emit('close')"
        class="px-4 py-2 bg-page-bg hover:bg-card-border text-text-primary text-xs font-bold rounded-xl transition-colors cursor-pointer"
      >
        Tutup
      </button>
    </template>
  </BaseModal>
</template>
