<script setup lang="ts">
import { computed, nextTick, onMounted, onUnmounted, ref, watch } from 'vue';
import type { PortfolioItem } from '../types/portfolio';

const props = defineProps<{
  item: PortfolioItem | null;
  isOpen: boolean;
  currentIndex?: number;
  totalItems?: number;
  hasPrev?: boolean;
  hasNext?: boolean;
}>();

const emit = defineEmits<{
  (e: 'close'): void;
  (e: 'prev'): void;
  (e: 'next'): void;
}>();

const previewImage = ref<string | null>(null);
const modalCardRef = ref<HTMLElement | null>(null);

// Reset pratinjau otomatis jika modal utama ditutup
watch(
  () => props.isOpen,
  (newVal) => {
    if (!newVal) {
      previewImage.value = null;
    }
  }
);

// Reset pratinjau dan kembalikan scroll modal ke atas saat berpindah proyek
watch(
  () => props.item?.id,
  async () => {
    previewImage.value = null;
    await nextTick();
    if (modalCardRef.value) {
      modalCardRef.value.scrollTop = 0;
    }
  }
);

const allImages = computed(() => {
  if (!props.item) return [];
  const list: string[] = [];
  if (props.item.imagePlaceholder) {
    list.push(props.item.imagePlaceholder);
  }
  if (props.item.galleryImages && props.item.galleryImages.length > 0) {
    list.push(...props.item.galleryImages);
  }
  return list;
});

const openPreview = (img: string) => {
  previewImage.value = img;
};

const closePreview = () => {
  previewImage.value = null;
};

const handleKeydown = (event: KeyboardEvent) => {
  if (!props.isOpen) return;

  if (event.key === 'Escape') {
    if (previewImage.value) {
      closePreview();
    } else {
      emit('close');
    }
  }
};

onMounted(() => {
  window.addEventListener('keydown', handleKeydown);
});

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown);
});
</script>

<template>
  <!-- Modal Utama -->
  <Transition
    enter-active-class="transition duration-300 ease-out"
    enter-from-class="opacity-0"
    enter-to-class="opacity-100"
    leave-active-class="transition duration-200 ease-in"
    leave-from-class="opacity-100"
    leave-to-class="opacity-0"
  >
    <div
      v-if="isOpen && item"
      class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/80 backdrop-blur-md overflow-y-auto"
      @click.self="emit('close')"
    >
      <div
        ref="modalCardRef"
        class="bg-[#1A1A1A] border border-white/10 rounded-2xl max-w-3xl w-full max-h-[90vh] overflow-y-auto p-6 md:p-8 shadow-2xl relative my-8"
      >
        <!-- Tombol Tutup Modal -->
        <button
          type="button"
          class="absolute top-5 right-5 text-white bg-white/15 hover:bg-white/25 border border-white/20 w-9 h-9 rounded-full flex items-center justify-center transition-colors"
          @click="emit('close')"
          aria-label="Tutup modal"
        >
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>

        <!-- Judul & Kategori -->
        <div class="mb-6 pr-10">
          <span
            class="inline-block px-3 py-1 text-xs tracking-wider uppercase rounded-full mb-3 font-medium"
            :class="
              item.category === 'TECH'
                ? 'bg-sky-400/10 text-sky-400 border border-sky-400/20'
                : 'bg-yellow-200/10 text-yellow-200 border border-yellow-200/20'
            "
          >
            {{ item.category === 'TECH' ? 'Rekayasa Perangkat Lunak' : 'Relasi Manusia & Bisnis' }}
          </span>
          <h2 class="text-2xl md:text-3xl font-serif text-white mb-1">
            {{ item.title }}
          </h2>
          <p class="text-sm text-yellow-200/80 font-sans">
            {{ item.subtitle }}
          </p>
        </div>

        <!-- Galeri Gambar Utuh (Tidak Terpotong) -->
        <div class="mb-8">
          <div class="flex items-center justify-between mb-3">
            <h3 class="text-xs uppercase tracking-wider text-gray-400 font-sans">
              Dokumentasi & Tangkapan Layar ({{ allImages.length }} Gambar)
            </h3>
            <span class="inline-flex items-center gap-1.5 text-[11px] text-yellow-200/75 font-sans">
              <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-4.35-4.35M11 19a8 8 0 100-16 8 8 0 000 16z" />
              </svg>
              <span>Ketuk gambar untuk perbesar</span>
            </span>
          </div>

          <div
            class="grid gap-4"
            :class="allImages.length === 1 ? 'grid-cols-1' : 'grid-cols-1 sm:grid-cols-2'"
          >
            <div
              v-for="(img, idx) in allImages"
              :key="idx"
              class="rounded-xl overflow-hidden border border-white/10 bg-[#111111] cursor-pointer group relative flex items-center justify-center min-h-[220px] max-h-[380px] p-3 hover:border-yellow-200/50 transition-all duration-300"
              @click="openPreview(img)"
            >
              <img
                :src="img"
                :alt="`${item.title} Foto ${idx + 1}`"
                class="w-auto h-auto max-w-full max-h-[350px] object-contain rounded-lg transition-transform duration-300 group-hover:scale-[1.02]"
                loading="lazy"
              />
              <div
                class="absolute bottom-3 right-3 bg-black/80 backdrop-blur-sm text-white text-[11px] px-2.5 py-1 rounded-md border border-white/20 opacity-85 group-hover:opacity-100 transition-opacity inline-flex items-center gap-1"
              >
                <span>Perbesar</span>
                <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 3h6v6M9 21H3v-6M21 3l-7 7M3 21l7-7" />
                </svg>
              </div>
            </div>
          </div>
        </div>

        <!-- Detail: Konteks & Masalah -->
        <div class="mb-6 bg-white/[0.02] border border-white/5 rounded-xl p-5">
          <h4 class="text-sm font-semibold text-white mb-2 flex items-center gap-2">
            <span class="w-2 h-2 rounded-full bg-sky-400"></span>
            Konteks Masalah & Realitas Lapangan
          </h4>
          <p class="text-gray-300 text-sm leading-relaxed font-sans">
            {{ item.sociologicalContext }}
          </p>
        </div>

        <!-- Detail: Solusi & Eksekusi -->
        <div class="mb-6 bg-white/[0.02] border border-white/5 rounded-xl p-5">
          <h4 class="text-sm font-semibold text-white mb-2 flex items-center gap-2">
            <span class="w-2 h-2 rounded-full bg-yellow-200"></span>
            Solusi Nyata & Eksekusi
          </h4>
          <p class="text-gray-300 text-sm leading-relaxed font-sans">
            {{ item.solutionExecution }}
          </p>
        </div>

        <!-- Keahlian / Tech Stack -->
        <div class="mb-6">
          <h4 class="text-xs uppercase tracking-wider text-gray-400 font-sans mb-3">
            Instrumen & Keahlian
          </h4>
          <div class="flex flex-wrap gap-2">
            <span
              v-for="(skill, idx) in item.stackOrSkills"
              :key="idx"
              class="px-3 py-1 bg-white/5 border border-white/10 rounded-lg text-xs text-gray-200"
            >
              {{ skill }}
            </span>
          </div>
        </div>

        <!-- Navigasi Proyek Sebelumnya / Selanjutnya -->
        <div
          v-if="hasPrev || hasNext"
          class="pt-5 border-t border-white/10 flex items-center justify-between gap-3"
        >
          <button
            type="button"
            class="px-4 py-2 rounded-full bg-white/5 hover:bg-white/10 border border-white/15 text-xs font-medium text-white hover:text-[#FEF08A] transition-colors inline-flex items-center gap-1.5"
            @click="emit('prev')"
          >
            <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
            </svg>
            <span>Proyek Sebelumnya</span>
          </button>

          <span
            v-if="totalItems && totalItems > 0"
            class="text-xs text-[#94A3B8] font-medium tabular-nums"
          >
            {{ (currentIndex ?? 0) + 1 }} / {{ totalItems }}
          </span>

          <button
            type="button"
            class="px-4 py-2 rounded-full bg-white/5 hover:bg-white/10 border border-white/15 text-xs font-medium text-white hover:text-[#FEF08A] transition-colors inline-flex items-center gap-1.5"
            @click="emit('next')"
          >
            <span>Proyek Selanjutnya</span>
            <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
            </svg>
          </button>
        </div>
      </div>
    </div>
  </Transition>

  <!-- Lightbox Overlay (Layar Penuh Saat Gambar Diketuk) -->
  <Transition
    enter-active-class="transition duration-200 ease-out"
    enter-from-class="opacity-0"
    enter-to-class="opacity-100"
    leave-active-class="transition duration-150 ease-in"
    leave-from-class="opacity-100"
    leave-to-class="opacity-0"
  >
    <div
      v-if="previewImage"
      class="fixed inset-0 z-[70] flex flex-col items-center justify-center p-4 bg-black/95 backdrop-blur-lg cursor-zoom-out"
      @click="closePreview"
    >
      <button
        type="button"
        class="absolute top-5 right-5 text-white bg-white/15 hover:bg-white/25 border border-white/20 w-10 h-10 rounded-full flex items-center justify-center transition-colors z-10"
        @click.stop="closePreview"
        aria-label="Tutup pratinjau"
      >
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
        </svg>
      </button>
      <img
        :src="previewImage"
        alt="Pratinjau Layar Penuh"
        class="max-w-[95vw] max-h-[85vh] object-contain rounded-lg shadow-2xl border border-white/10"
      />
      <p class="text-xs text-gray-300 mt-3 font-sans">
        Ketuk di mana saja atau tekan Esc untuk menutup
      </p>
    </div>
  </Transition>
</template>
