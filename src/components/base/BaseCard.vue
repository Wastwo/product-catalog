<script setup></script>

<template>
  <div
    class="bg-white rounded-xl border border-card-border overflow-hidden shadow-sm hover:shadow-md hover:-translate-y-1 transition-all duration-200 flex flex-col justify-between h-full"
  >
    <div v-if="$slots.header" class="p-3 border-b border-card-border bg-page-bg/50">
      <slot name="header" />
    </div>

    <div v-if="$slots.media" class="relative w-full overflow-hidden">
      <slot name="media" />
    </div>

    <div class="p-3 flex-1 flex flex-col justify-between">
      <slot />
    </div>

    <div v-if="$slots.footer" class="p-3 pt-2 border-t border-card-border mt-auto">
      <slot name="footer" />
    </div>
  </div>
</template>
