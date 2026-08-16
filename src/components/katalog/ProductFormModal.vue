<script setup>
import { ref, watch } from 'vue'
import BaseModal from '../base/BaseModal.vue'

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false,
  },
})

const emit = defineEmits(['close', 'add-product'])

const initialFormState = {
  nama: '',
  harga: '',
  kategori: 'Fashion',
  stok: 10,
  deskripsi: '',
  gambar:
    'https://images.unsplash.com/photo-1523275335684-37898b6baf30?auto=format&fit=crop&q=80&w=600',
  lokasi: 'Jakarta',
  rating: 5.0,
  terjual: 0,
}

const formData = ref({ ...initialFormState })
const errorMessage = ref('')

watch(
  () => props.isOpen,
  (newVal) => {
    if (newVal) {
      formData.value = { ...initialFormState }
      errorMessage.value = ''
    }
  },
)

function handleSubmit() {
  if (!formData.value.nama.trim()) {
    errorMessage.value = 'Nama produk tidak boleh kosong!'
    return
  }
  if (!formData.value.harga || formData.value.harga <= 0) {
    errorMessage.value = 'Harga produk harus lebih besar dari 0!'
    return
  }

  emit('add-product', {
    ...formData.value,
    id: Date.now(),
    nama: formData.value.nama.trim(),
    harga: Number(formData.value.harga),
    stok: Number(formData.value.stok),
  })

  formData.value = { ...initialFormState }
  errorMessage.value = ''
  emit('close')
}
</script>

<template>
  <BaseModal :is-open="isOpen" title="➕ Tambah Produk Baru" @close="$emit('close')">
    <form
      @submit.prevent="handleSubmit"
      id="add-product-form"
      class="space-y-4 font-sans text-xs md:text-sm"
    >
      <div
        v-if="errorMessage"
        class="p-3 bg-rose-50 border border-rose-200 text-rose-600 rounded-xl font-medium"
      >
        {{ errorMessage }}
      </div>

      <div class="flex flex-col justify-center gap-1">
        <label class="block font-bold text-text-primary">Nama Produk *</label>
        <input
          v-model="formData.nama"
          type="text"
          placeholder="Contoh: Sepatu Sneakers Original"
          class="w-full px-3 py-2 border border-card-border rounded-xl focus:outline-none focus:border-brand-primary"
        />
      </div>

      <div class="grid grid-cols-2 gap-3">
        <div class="flex flex-col justify-center gap-1">
          <label class="block font-bold text-text-primary">Harga (Rp) *</label>
          <input
            v-model.number="formData.harga"
            type="number"
            placeholder="50000"
            class="w-full px-3 py-2 border border-card-border rounded-xl focus:outline-none focus:border-brand-primary"
          />
        </div>
        <div class="flex flex-col justify-center gap-1">
          <label class="block font-bold text-text-primary">Stok *</label>
          <input
            v-model.number="formData.stok"
            type="number"
            placeholder="10"
            class="w-full px-3 py-2 border border-card-border rounded-xl focus:outline-none focus:border-brand-primary"
          />
        </div>
      </div>

      <div class="flex flex-col justify-center gap-1">
        <label class="block font-bold text-text-primary">Kategori *</label>
        <select
          v-model="formData.kategori"
          class="w-full px-3 py-2 border border-card-border rounded-xl focus:outline-none focus:border-brand-primary bg-white"
        >
          <option value="Fashion">Fashion</option>
          <option value="Elektronik">Elektronik</option>
          <option value="Aksesoris">Aksesoris</option>
        </select>
      </div>

      <div class="flex flex-col justify-center gap-1">
        <label class="block font-bold text-text-primary">Deskripsi Produk</label>
        <textarea
          v-model="formData.deskripsi"
          rows="3"
          placeholder="Tulis deskripsi singkat tentang produk..."
          class="w-full px-3 py-2 border border-card-border rounded-xl focus:outline-none focus:border-brand-primary"
        ></textarea>
      </div>
    </form>

    <template #footer>
      <button
        type="button"
        @click="$emit('close')"
        class="px-4 py-2 bg-slate-100 hover:bg-slate-200 text-text-primary text-xs font-bold rounded-xl transition-colors cursor-pointer"
      >
        Batal
      </button>
      <button
        type="submit"
        form="add-product-form"
        class="px-4 py-2 bg-brand-primary hover:bg-emerald-600 active:bg-emerald-700 text-white text-xs font-bold rounded-xl transition-colors shadow-sm cursor-pointer"
      >
        Simpan Produk
      </button>
    </template>
  </BaseModal>
</template>
