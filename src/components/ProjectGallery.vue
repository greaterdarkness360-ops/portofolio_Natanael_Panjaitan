&lt;template&gt;

  &lt;section

    id=&quot;works&quot;

    ref=&quot;sectionRef&quot;

    class=&quot;py-24 px-6 max-w-6xl mx-auto border-t border-white/5 transition-all duration-700 ease-out&quot;

    :class=&quot;isVisible ? &#39;opacity-100 translate-y-0&#39; : &#39;opacity-0 translate-y-6&#39;&quot;

  &gt;

    &lt;!-- Header --&gt;

    &lt;div class=&quot;flex flex-col md:flex-row md:items-end justify-between mb-10 gap-6&quot;&gt;

      &lt;div&gt;

        &lt;div class=&quot;inline-flex items-center gap-2 px-3 py-1 rounded-full border border-[#86EFAC]/20 bg-[#1A1A1A] text-[#86EFAC] text-xs font-semibold uppercase tracking-widest mb-3&quot;&gt;

          Portofolio Terpilih

        &lt;/div&gt;

        &lt;h2 class=&quot;text-3xl sm:text-4xl lg:text-5xl font-serif font-semibold text-[#F8FAFC]&quot;&gt;

          Karya &amp; Eksekusi Sistem

        &lt;/h2&gt;

      &lt;/div&gt;



      <!-- Filter Buttons -->

      <div class="flex flex-wrap gap-2">

        <button

          v-for="cat in categories"

          :key="cat.key"

          @click="selectCategory(cat.key)"

          class="px-4 py-2 rounded-full text-xs font-medium transition-all duration-300"

          :class="activeCategory === cat.key

            ? 'bg-[#38BDF8] text-[#121212] font-semibold shadow-[0_0_15px_rgba(56,189,248,0.3)]'

            : 'bg-[#1A1A1A] text-[#94A3B8] hover:text-[#F8FAFC] border border-white/10 hover:border-white/20'"

        >

          {{ cat.label }}

        </button>

      </div>

    </div>



    <!-- Hybrid Part 1: Sorotan Terpilih (Terinventarisasi dengan Prev / Next) -->

    <div v-if="featuredItem" class="mb-14">

      <div class="flex flex-wrap items-center justify-between gap-4 mb-4">

        <div class="flex items-center gap-2.5">

          <span class="w-2 h-2 rounded-full bg-[#38BDF8]"></span>

          <h3 class="text-xs uppercase tracking-widest text-[#94A3B8] font-semibold">

            Sorotan Terpilih

          </h3>

        </div>



        <div class="flex items-center gap-2">

          <button

            type="button"

            @click="prevFeatured"

            class="px-3.5 py-1.5 rounded-full bg-[#1A1A1A] border border-white/10 hover:border-[#38BDF8]/50 text-xs font-medium text-[#F8FAFC] hover:text-[#FEF08A] transition-colors inline-flex items-center gap-1.5"

            aria-label="Proyek sorotan sebelumnya"

          >

            <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">

              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />

            </svg>

            <span>Sebelumnya</span>

          </button>



          <span class="px-3 py-1 rounded-full bg-[#1A1A1A] border border-white/10 text-xs font-medium text-[#94A3B8] tabular-nums">

            {{ featuredIndex + 1 }} / {{ filteredItems.length }}

          </span>



          <button

            type="button"

            @click="nextFeatured"

            class="px-3.5 py-1.5 rounded-full bg-[#1A1A1A] border border-white/10 hover:border-[#38BDF8]/50 text-xs font-medium text-[#F8FAFC] hover:text-[#FEF08A] transition-colors inline-flex items-center gap-1.5"

            aria-label="Proyek sorotan selanjutnya"

          >

            <span>Selanjutnya</span>

            <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">

              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />

            </svg>

          </button>

        </div>

      </div>



      <Transition

        mode="out-in"

        enter-active-class="transition duration-300 ease-out"

        enter-from-class="opacity-0 translate-y-2"

        enter-to-class="opacity-100 translate-y-0"

        leave-active-class="transition duration-200 ease-in"

        leave-from-class="opacity-100 translate-y-0"

        leave-to-class="opacity-0 -translate-y-2"

      >

        <div

          :key="featuredItem.id"

          class="rounded-2xl bg-[#1A1A1A] border border-white/10 hover:border-[#86EFAC]/30 transition-all duration-500 overflow-hidden shadow-xl grid grid-cols-1 lg:grid-cols-12"

        >

          <!-- Gambar Pratinjau Kiri -->

          <div

            class="lg:col-span-5 bg-[#111111] p-4 flex items-center justify-center min-h-[240px] lg:min-h-[320px] cursor-pointer group border-b lg:border-b-0 lg:border-r border-white/5"

            @click="openModal(featuredItem)"

          >

            <img

              :src="featuredItem.imagePlaceholder"

              :alt="featuredItem.title"

              class="w-full max-h-[280px] object-contain rounded-xl transition-transform duration-500 group-hover:scale-[1.03]"

              loading="lazy"

            />

          </div>



          <!-- Konten Sorotan Kanan -->

          <div class="lg:col-span-7 p-6 sm:p-8 flex flex-col justify-between gap-5">

            <div>

              <div class="flex flex-wrap items-center justify-between gap-2 mb-4">

                <span

                  class="px-3 py-1 rounded-full text-[11px] font-medium tracking-wide border"

                  :class="featuredItem.category === 'TECH'

                    ? 'bg-[#38BDF8]/10 text-[#38BDF8] border-[#38BDF8]/20'

                    : 'bg-[#FEF08A]/10 text-[#FEF08A] border-[#FEF08A]/20'"

                >

                  {{ featuredItem.category === 'TECH' ? 'Rekayasa Perangkat Lunak' : 'Relasi Manusia & Bisnis' }}

                </span>

                <span v-if="featuredItem.highlightMetric" class="text-xs text-[#86EFAC] font-medium">

                  {{ featuredItem.highlightMetric }}

                </span>

              </div>



              <h3 class="text-2xl sm:text-3xl font-serif font-semibold text-[#F8FAFC] leading-snug mb-2">

                {{ featuredItem.title }}

              </h3>

              <p class="text-sm text-[#FEF08A]/85 mb-4">

                {{ featuredItem.subtitle }}

              </p>

              <p class="text-xs sm:text-sm text-[#94A3B8] leading-relaxed line-clamp-3">

                {{ featuredItem.sociologicalContext }}

              </p>

            </div>



            <div class="flex flex-wrap items-center justify-between gap-4 pt-2 border-t border-white/5">

              <div class="flex flex-wrap gap-1.5">

                <span

                  v-for="skill in featuredItem.stackOrSkills.slice(0, 4)"

                  :key="skill"

                  class="px-2.5 py-1 rounded-md bg-[#121212] border border-white/5 text-[11px] text-[#94A3B8]"

                >

                  {{ skill }}

                </span>

              </div>



              <button

                type="button"

                @click="openModal(featuredItem)"

                class="px-5 py-2.5 rounded-full bg-[#38BDF8] text-[#121212] font-semibold text-xs hover:bg-[#FEF08A] transition-colors inline-flex items-center gap-1.5 shrink-0"

              >

                <span>Buka Detail</span>

                <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">

                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />

                </svg>

              </button>

            </div>

          </div>

        </div>

      </Transition>

    </div>



    <!-- Hybrid Part 2: Arsip Lengkap (Full Bento Grid) -->

    <div class="flex items-center justify-between mb-6">

      <div class="flex items-center gap-2.5">

        <span class="w-2 h-2 rounded-full bg-[#86EFAC]"></span>

        <h3 class="text-xs uppercase tracking-widest text-[#94A3B8] font-semibold">

          Arsip Lengkap Karya ({{ filteredItems.length }})

        </h3>

      </div>

    </div>



    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">

      <div

        v-for="(item, index) in filteredItems"

        :key="item.id"

        @click="openModal(item)"

        class="group relative rounded-2xl bg-[#1A1A1A] border border-white/10 hover:border-[#86EFAC]/30 p-6 flex flex-col justify-between cursor-pointer transition-all duration-500 hover:scale-[1.03] hover:-translate-y-1 hover:shadow-[0_10px_30px_rgba(0,0,0,0.5)] overflow-hidden"

        :class="{

          'md:col-span-2 lg:col-span-2': index === 0 && activeCategory === 'ALL',

        }"

      >

        <!-- Subtle Glow Background Effect on Hover -->

        <div class="absolute -inset-px bg-gradient-to-r from-[#38BDF8]/0 via-[#FEF08A]/0 to-[#86EFAC]/0 group-hover:from-[#38BDF8]/10 group-hover:via-[#FEF08A]/5 group-hover:to-[#86EFAC]/10 rounded-2xl transition-all duration-500 pointer-events-none"></div>



        <div>

          <!-- Top Row: Category Badge & Metric -->

          <div class="flex items-center justify-between gap-2 mb-4">

            <span

              class="px-3 py-1 rounded-full text-[11px] font-medium tracking-wide border"

              :class="item.category === 'TECH'

                ? 'bg-[#38BDF8]/10 text-[#38BDF8] border-[#38BDF8]/20'

                : 'bg-[#FEF08A]/10 text-[#FEF08A] border-[#FEF08A]/20'"

            >

              {{ item.category === 'TECH' ? 'Rekayasa Perangkat Lunak' : 'Relasi Manusia & Bisnis' }}

            </span>

            <span v-if="item.highlightMetric" class="text-[11px] text-[#86EFAC] font-medium text-right">

              {{ item.highlightMetric }}

            </span>

          </div>



          <!-- Title & Subtitle -->

          <h3 class="text-xl font-serif font-semibold text-[#F8FAFC] group-hover:text-[#FEF08A] transition-colors duration-300 leading-snug mb-2">

            {{ item.title }}

          </h3>

          <p class="text-xs sm:text-sm text-[#94A3B8] mb-6">

            {{ item.subtitle }}

          </p>

        </div>



        <!-- Card Bottom: Stack Tags & Action -->

        <div>

          <div class="flex flex-wrap gap-1.5 mb-4">

            <span

              v-for="skill in item.stackOrSkills.slice(0, 3)"

              :key="skill"

              class="px-2.5 py-0.5 rounded-md bg-[#121212] border border-white/5 text-[11px] text-[#94A3B8]"

            >

              {{ skill }}

            </span>

            <span v-if="item.stackOrSkills.length > 3" class="px-2 py-0.5 text-[11px] text-[#94A3B8]">

              +{{ item.stackOrSkills.length - 3 }}

            </span>

          </div>



          <div class="flex items-center gap-1 text-xs font-semibold text-[#38BDF8] group-hover:text-[#FEF08A] transition-colors duration-300">

            <span>Pelajari Lebih Lanjut</span>

            <span class="transform group-hover:translate-x-1 transition-transform duration-300">→</span>

          </div>

        </div>

      </div>

    </div>



    <!-- Modal Component -->

    <ProjectModal

      :is-open="isModalOpen"

      :item="selectedItem"

      :current-index="selectedIndex"

      :total-items="filteredItems.length"

      :has-prev="filteredItems.length > 1"

      :has-next="filteredItems.length > 1"

      @close="closeModal"

      @prev="navigateModal(-1)"

      @next="navigateModal(1)"

    />

  &lt;/section&gt;

&lt;/template&gt;



&lt;script setup lang=&quot;ts&quot;&gt;

import { computed, onMounted, onUnmounted, ref } from &#39;vue&#39;;

import { portfolioItems } from &#39;../data/portfolioData&#39;;

import type { PortfolioItem, ProjectCategory } from &#39;../types/portfolio&#39;;

import ProjectModal from &#39;./ProjectModal.vue&#39;;



const activeCategory = ref&lt;ProjectCategory&gt;(&#39;ALL&#39;);

const featuredIndex = ref(0);

const isModalOpen = ref(false);

const selectedIndex = ref(0);

const sectionRef = ref&lt;HTMLElement | null&gt;(null);

const isVisible = ref(false);

let observer: IntersectionObserver | null = null;



const categories = [

  { key: &#39;ALL&#39; as ProjectCategory, label: &#39;Semua Karya&#39; },

  { key: &#39;HUMAN_OPS&#39; as ProjectCategory, label: &#39;Relasi Manusia &amp; Bisnis&#39; },

  { key: &#39;TECH&#39; as ProjectCategory, label: &#39;Rekayasa Perangkat Lunak&#39; },

];



const filteredItems = computed(() =&gt; {

  if (activeCategory.value === &#39;ALL&#39;) return portfolioItems;

  return portfolioItems.filter(item =&gt; item.category === activeCategory.value);

});



const featuredItem = computed&lt;PortfolioItem | null&gt;(() =&gt; {

  if (filteredItems.value.length === 0) return null;

  const safeIndex = featuredIndex.value % filteredItems.value.length;

  return filteredItems.value[safeIndex] ?? filteredItems.value[0];

});



const selectedItem = computed&lt;PortfolioItem | null&gt;(() =&gt; {

  if (!isModalOpen.value || filteredItems.value.length === 0) return null;

  return filteredItems.value[selectedIndex.value] ?? null;

});



const selectCategory = (category: ProjectCategory) =&gt; {

  activeCategory.value = category;

  featuredIndex.value = 0;

  selectedIndex.value = 0;

};



const prevFeatured = () =&gt; {

  const total = filteredItems.value.length;

  if (total === 0) return;

  featuredIndex.value = (featuredIndex.value - 1 + total) % total;

};



const nextFeatured = () =&gt; {

  const total = filteredItems.value.length;

  if (total === 0) return;

  featuredIndex.value = (featuredIndex.value + 1) % total;

};



const openModal = (item: PortfolioItem) =&gt; {

  const idx = filteredItems.value.findIndex(entry =&gt; entry.id === item.id);

  selectedIndex.value = idx &gt;= 0 ? idx : 0;

  isModalOpen.value = true;

};



const closeModal = () =&gt; {

  isModalOpen.value = false;

};



const navigateModal = (direction: number) =&gt; {

  const total = filteredItems.value.length;

  if (total &lt;= 1) return;

  selectedIndex.value = (selectedIndex.value + direction + total) % total;

};



onMounted(() =&gt; {

  if (typeof IntersectionObserver !== &#39;undefined&#39; &amp;&amp; sectionRef.value) {

    observer = new IntersectionObserver(

      (entries) =&gt; {

        if (entries[0]?.isIntersecting) {

          isVisible.value = true;

          observer?.disconnect();

        }

      },

      { threshold: 0.05 }

    );

    observer.observe(sectionRef.value);

  } else {

    isVisible.value = true;

  }

});



onUnmounted(() =&gt; {

  observer?.disconnect();

});

&lt;/script&gt;

