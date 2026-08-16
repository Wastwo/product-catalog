<script setup>
defineProps({
  isOpen: {
    type: Boolean,
    default: false,
  },
  title: {
    type: String,
    default: '',
  },
})

defineEmits(['close'])
</script>

<template>
  <Teleport to="body">
    <Transition
      enter-active-class="transition duration-200 ease-out"
      enter-from-class="opacity-0"
      enter-to-class="opacity-100"
      leave-active-class="transition duration-150 ease-in"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <div
        v-if="isOpen"
        class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-900/50 backdrop-blur-sm"
        @click.self="$emit('close')"
      >
        <div
          class="bg-white rounded-2xl shadow-xl border border-card-border w-full max-w-lg overflow-hidden flex flex-col max-h-[90vh]"
        >
          <div
            class="flex items-center justify-between p-4 border-b border-card-border bg-page-bg/40"
          >
            <slot name="header">
              <h3 class="font-display font-bold text-base md:text-lg text-text-primary">
                {{ title }}
              </h3>
            </slot>
            <button
              @click="$emit('close')"
              class="text-text-muted hover:text-text-primary p-1.5 rounded-lg hover:bg-slate-100 transition-colors cursor-pointer"
              aria-label="Tutup modal"
            >
              <svg
                class="w-5 h-5"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                stroke-width="2"
              >
                <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>

          <div class="p-5 overflow-y-auto flex-1 font-sans text-sm">
            <slot />
          </div>

          <div
            v-if="$slots.footer"
            class="p-4 border-t border-card-border bg-page-bg/40 flex justify-end gap-2"
          >
            <slot name="footer" />
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>
