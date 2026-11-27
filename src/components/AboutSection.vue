&lt;script setup lang=&quot;ts&quot;&gt;

import { onMounted, onUnmounted, ref } from &#39;vue&#39;;



const coreSkills = [

  &#39;Analisis Sosial &amp; Interpersonal&#39;,

  &#39;Komunikasi Strategis&#39;,

  &#39;B2C Operations&#39;,

  &#39;AI-Augmented Engineering&#39;,

  &#39;AI System Orchestration&#39;

];



interface ActivityCard {

  id: string;

  type: &#39;image&#39; | &#39;video&#39;;

  src: string;

  title: string;

  subtitle: string;

  previewTitle: string;

  dotColor: string;

  hoverBorder: string;

  badgeText: string;

  badgeClass: string;

}



const activityCards: ActivityCard[] = [

  {

    id: &#39;riset-kualitatif&#39;,

    type: &#39;video&#39;,

    src: &#39;./assets/field-research.mp4&#39;,

    title: &#39;Riset Kualitatif&#39;,

    subtitle: &#39;Wawancara Warga&#39;,

    previewTitle: &#39;Riset Kualitatif: Observasi &amp; Wawancara Mendalam bersama Warga&#39;,

    dotColor: &#39;bg-emerald-400 animate-pulse&#39;,

    hoverBorder: &#39;hover:border-emerald-400/40&#39;,

    badgeText: &#39;Perbesar Video&#39;,

    badgeClass: &#39;bg-emerald-500/80 border-white/20 backdrop-blur-sm font-medium&#39;

  },

  {

    id: &#39;diskusi-kualitatif&#39;,

    type: &#39;video&#39;,

    src: &#39;./assets/team-discussion.mp4&#39;,

    title: &#39;Diskusi Kualitatif&#39;,

    subtitle: &#39;Briefing Tim&#39;,

    previewTitle: &#39;Diskusi Kualitatif: Koordinasi &amp; Briefing Tim Peneliti di Lapangan&#39;,

    dotColor: &#39;bg-sky-400 animate-pulse&#39;,

    hoverBorder: &#39;hover:border-sky-400/40&#39;,

    badgeText: &#39;Perbesar Video&#39;,

    badgeClass: &#39;bg-sky-500/80 border-white/20 backdrop-blur-sm font-medium&#39;

  },

  {

    id: &#39;operasional-relasi&#39;,

    type: &#39;image&#39;,

    src: &#39;./assets/activity-2.jpg&#39;,

    title: &#39;Operasional &amp; Relasi&#39;,

    subtitle: &#39;Layanan B2C&#39;,

    previewTitle: &#39;Operasional &amp; Relasi Pelanggan E-Commerce&#39;,

    dotColor: &#39;bg-yellow-200&#39;,

    hoverBorder: &#39;hover:border-yellow-200/40&#39;,

    badgeText: &#39;Perbesar&#39;,

    badgeClass: &#39;bg-black/75 border-white/15&#39;

  },

  {

    id: &#39;eksplorasi-sistem&#39;,

    type: &#39;image&#39;,

    src: &#39;./assets/activity-3.jpg&#39;,

    title: &#39;Eksplorasi Sistem&#39;,

    subtitle: &#39;Otomasi &amp; Kode&#39;,

    previewTitle: &#39;Eksplorasi Sistem &amp; Rekayasa Perangkat Lunak&#39;,

    dotColor: &#39;bg-purple-400&#39;,

    hoverBorder: &#39;hover:border-purple-400/40&#39;,

    badgeText: &#39;Perbesar&#39;,

    badgeClass: &#39;bg-black/75 border-white/15&#39;

  }

];



interface MediaPreview {

  type: &#39;image&#39; | &#39;video&#39;;

  src: string;

  title: string;

}



const activePreview = ref&lt;MediaPreview | null&gt;(null);

const sectionRef = ref&lt;HTMLElement | null&gt;(null);

const isVisible = ref(false);

let observer: IntersectionObserver | null = null;



const openPreview = (card: ActivityCard) =&gt; {

  activePreview.value = {

    type: card.type,

    src: card.src,

    title: card.previewTitle

  };

};



const closePreview = () =&gt; {

  activePreview.value = null;

};



const handleKeydown = (event: KeyboardEvent) =&gt; {

  if (event.key === &#39;Escape&#39; &amp;&amp; activePreview.value) {

    closePreview();

  }

};



onMounted(() =&gt; {

  window.addEventListener(&#39;keydown&#39;, handleKeydown);



  if (typeof IntersectionObserver !== &#39;undefined&#39; &amp;&amp; sectionRef.value) {

    observer = new IntersectionObserver(

      (entries) =&gt; {

        if (entries[0]?.isIntersecting) {

          isVisible.value = true;

          observer?.disconnect();

        }

      },

      { threshold: 0.08 }

    );

    observer.observe(sectionRef.value);

  } else {

    isVisible.value = true;

  }

});



onUnmounted(() =&gt; {

  window.removeEventListener(&#39;keydown&#39;, handleKeydown);

  observer?.disconnect();

});

&lt;/script&gt;



&lt;template&gt;

  &lt;section

    id=&quot;about&quot;

    ref=&quot;sectionRef&quot;

    class=&quot;py-20 px-4 max-w-6xl mx-auto transition-all duration-700 ease-out&quot;

    :class=&quot;isVisible ? &#39;opacity-100 translate-y-0&#39; : &#39;opacity-0 translate-y-6&#39;&quot;

  &gt;

    &lt;!-- Header Section --&gt;

    &lt;div class=&quot;mb-12&quot;&gt;

      &lt;span class=&quot;text-xs uppercase tracking-widest text-yellow-200/80 font-sans font-medium&quot;&gt;

        Kilas Balik &amp; Fondasi Kerja

      &lt;/span&gt;

      &lt;h2 class=&quot;text-3xl md:text-4xl font-sans font-bold tracking-tight text-white mt-1&quot;&gt;

        Tentang Diri &amp; Realitas Lapangan

      &lt;/h2&gt;

    &lt;/div&gt;



    <div class="grid grid-cols-1 lg:grid-cols-12 gap-10 items-start">

      <!-- Kolom Kiri: Narasi Profil (3 Paragraf Seimbang) & Skill Tags -->

      <div class="lg:col-span-5 space-y-5">

        <p class="text-gray-300 font-sans leading-relaxed text-sm md:text-base">

          Berangkat dari latar belakang Sosiologi dan pemahaman interaksi manusia, saya memadukan kepekaan riset kualitatif dengan ketangkasan eksekusi teknis modern. Bagi saya, teknologi dan strategi bisnis bukan sekadar baris kode atau target angka, melainkan sarana untuk merespons kebutuhan nyata manusia di lapangan.

        </p>

        <p class="text-gray-300 font-sans leading-relaxed text-sm md:text-base">

          Pengalaman turun langsung ke pemukiman warga untuk menggali data, berkoordinasi dengan tim di medan terbuka, hingga menghadapi ratusan pesan pembeli di operasional toko daring membentuk cara kerja yang tenang, adaptif, dan solutif. Setiap keberatan pelanggan maupun dinamika kelompok selalu saya dekati dengan komunikasi yang menenangkan dan negosiasi yang adil.

        </p>

        <p class="text-gray-400 font-sans leading-relaxed text-sm">

          Empati sosial tersebut kemudian diterjemahkan ke dalam rancangan sistem digital yang ringkas dan tepat guna. Ketika alur kerja manual terasa lambat atau aplikasi yang ada terlalu rumit bagi pengguna awam, saya merancang dan mengotomatisasi solusinya secara mandiri agar operasional harian berjalan lebih efisien, transparan, dan berdampak nyata.

        </p>



        <!-- Skill Tags -->

        <div class="pt-3">

          <h3 class="text-xs uppercase tracking-wider text-gray-400 font-sans mb-3 font-semibold">

            Fokus Kompetensi

          </h3>

          <div class="flex flex-wrap gap-2">

            <span

              v-for="(skill, index) in coreSkills"

              :key="index"

              class="px-3 py-1.5 rounded-lg text-xs font-sans bg-white/5 border border-white/10 text-gray-200 hover:border-yellow-200/40 transition-colors"

            >

              {{ skill }}

            </span>

          </div>

        </div>

      </div>



      <!-- Kolom Kanan: Galeri Aktivitas Lapangan & Teknis (2x2 Grid Utuh) -->

      <div class="lg:col-span-7 space-y-3">

        <div class="flex items-center justify-between">

          <span class="text-xs uppercase tracking-wider text-gray-400 font-sans">

            Dokumentasi Aktivitas Nyata

          </span>

          <span class="inline-flex items-center gap-1.5 text-[11px] text-yellow-200/80 font-sans">

            <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">

              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-4.35-4.35M11 19a8 8 0 100-16 8 8 0 000 16z" />

            </svg>

            <span>Ketuk kartu untuk memperbesar</span>

          </span>

        </div>



        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">

          <div

            v-for="card in activityCards"

            :key="card.id"

            class="rounded-2xl overflow-hidden border border-white/10 bg-[#141414] transition-all duration-300 flex flex-col cursor-pointer group shadow-lg"

            :class="card.hoverBorder"

            @click="openPreview(card)"

          >

            <div class="w-full h-56 bg-[#0d0d0d] flex items-center justify-center p-2 overflow-hidden relative">

              <img

                v-if="card.type === 'image'"

                :src="card.src"

                :alt="card.title"

                class="w-full h-full object-contain rounded-lg transition-transform duration-500 group-hover:scale-[1.03]"

                loading="lazy"

              />

              <video

                v-else

                :src="card.src"

                autoplay

                loop

                muted

                playsinline

                class="w-full h-full object-contain rounded-lg transition-transform duration-500 group-hover:scale-[1.03]"

              ></video>

              <span

                class="absolute top-3 right-3 text-white text-[10px] px-2.5 py-1 rounded-md border inline-flex items-center gap-1"

                :class="card.badgeClass"

              >

                <svg

                  v-if="card.type === 'video'"

                  class="w-3 h-3"

                  fill="currentColor"

                  viewBox="0 0 24 24"

                  aria-hidden="true"

                >

                  <path d="M8 5v14l11-7z" />

                </svg>

                <svg

                  v-else

                  class="w-3 h-3"

                  fill="none"

                  stroke="currentColor"

                  viewBox="0 0 24 24"

                  aria-hidden="true"

                >

                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 3h6v6M9 21H3v-6M21 3l-7 7M3 21l7-7" />

                </svg>

                <span>{{ card.badgeText }}</span>

              </span>

            </div>



            <div class="p-3.5 bg-[#1A1A1A] border-t border-white/5 flex items-center justify-between gap-2">

              <div class="flex items-center gap-2">

                <span class="w-2 h-2 rounded-full shrink-0" :class="card.dotColor"></span>

                <span class="text-xs font-medium text-white font-sans">{{ card.title }}</span>

              </div>

              <span class="text-[11px] text-gray-400 font-sans">{{ card.subtitle }}</span>

            </div>

          </div>

        </div>

      </div>

    </div>



    <!-- Blok Lebar Penuh: Kredensial & Kepemimpinan -->

    <div class="mt-14 pt-12 border-t border-white/5">

      <div class="mb-6">

        <span class="text-xs uppercase tracking-widest text-[#86EFAC]/80 font-sans font-medium">

          Validasi Kompetensi & Rekam Jejak

        </span>

        <h3 class="text-2xl md:text-3xl font-serif font-semibold text-[#F8FAFC] mt-1">

          Kredensial & Kepemimpinan

        </h3>

      </div>



      <div class="grid grid-cols-1 md:grid-cols-3 gap-5">

        <!-- Kartu 1: IBM SkillsBuild -->

        <div class="rounded-2xl bg-[#1A1A1A] border border-white/10 hover:border-[#38BDF8]/40 p-6 transition-all duration-300 flex flex-col justify-between">

          <div>

            <div class="flex items-center justify-between gap-2 mb-3">

              <span class="px-2.5 py-1 rounded-full text-[11px] font-medium bg-[#38BDF8]/10 text-[#38BDF8] border border-[#38BDF8]/20">

                IBM SkillsBuild

              </span>

              <span class="text-[11px] text-[#94A3B8]">Nov 2026</span>

            </div>

            <h4 class="text-lg font-serif font-semibold text-[#F8FAFC] mb-2">

              Data Lifecycle & IBM Granite AI

            </h4>

            <p class="text-xs sm:text-sm text-[#94A3B8] leading-relaxed mb-4">

              Sertifikasi kompetensi global <strong class="text-[#F8FAFC] font-medium">Getting Started with Data</strong> dan <strong class="text-[#F8FAFC] font-medium">Classifying Data Using IBM Granite</strong> untuk pembersihan data, klasifikasi dokumen administrasi, serta analisis sentimen pelanggan.

            </p>

          </div>

          <a

            href="https://www.credly.com/badges/ddd309e9-7d7d-4d82-8af7-3286514bbdae"

            target="_blank"

            rel="noopener noreferrer"

            class="inline-flex items-center gap-1.5 text-xs font-semibold text-[#38BDF8] hover:text-[#FEF08A] transition-colors"

          >

            <span>Lihat Lencana Terverifikasi Credly</span>

            <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">

              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 3h7v7m0-7L10 14M5 5v14h14v-5" />

            </svg>

          </a>

        </div>



        <!-- Kartu 2: Skillpedia Indonesia -->

        <div class="rounded-2xl bg-[#1A1A1A] border border-white/10 hover:border-[#FEF08A]/40 p-6 transition-all duration-300 flex flex-col justify-between">

          <div>

            <div class="flex items-center justify-between gap-2 mb-3">

              <span class="px-2.5 py-1 rounded-full text-[11px] font-medium bg-[#FEF08A]/10 text-[#FEF08A] border border-[#FEF08A]/20">

                Skillpedia Indonesia

              </span>

              <span class="text-[11px] text-[#94A3B8]">Okt 2026</span>

            </div>

            <h4 class="text-lg font-serif font-semibold text-[#F8FAFC] mb-2">

              Microsoft Excel & Gemini AI (Predikat A)

            </h4>

            <p class="text-xs sm:text-sm text-[#94A3B8] leading-relaxed mb-4">

              Lulus Predikat A (Sangat Baik) pada kelas <strong class="text-[#F8FAFC] font-medium">Microsoft Excel</strong> (rumus logika, database administrasi, payroll) serta <strong class="text-[#F8FAFC] font-medium">Gemini Canvas & AI Produktivitas</strong>, dan meraih penghargaan Peserta Terbaik.

            </p>

          </div>

          <span class="text-[11px] text-[#86EFAC] font-medium">

            Peserta Terbaik Predikat A (Sangat Baik)

          </span>

        </div>



        <!-- Kartu 3: Kepemimpinan & Organisasi -->

        <div class="rounded-2xl bg-[#1A1A1A] border border-white/10 hover:border-[#86EFAC]/40 p-6 transition-all duration-300 flex flex-col justify-between">

          <div>

            <div class="flex items-center justify-between gap-2 mb-3">

              <span class="px-2.5 py-1 rounded-full text-[11px] font-medium bg-[#86EFAC]/10 text-[#86EFAC] border border-[#86EFAC]/20">

                Organisasi & Relawan

              </span>

              <span class="text-[11px] text-[#94A3B8]">2022 sampai 2025</span>

            </div>

            <h4 class="text-lg font-serif font-semibold text-[#F8FAFC] mb-2">

              Kepemimpinan Lintas Divisi & Edukasi Publik

            </h4>

            <p class="text-xs sm:text-sm text-[#94A3B8] leading-relaxed mb-4">

              Wakil Ketua Panitia & Koordinator Acara Turnamen Debat Daring Mahasiswa dan Perayaan Natal (2024 sampai 2025), serta Project Leader dan Pembicara Utama Kampanye Anti-Perundungan di SMA Methodist 12 Medan di hadapan 100+ siswa.

            </p>

          </div>

          <span class="text-[11px] text-[#86EFAC] font-medium">

            Koordinasi Lintas Seksi & 100+ Audiens

          </span>

        </div>

      </div>

    </div>



    <!-- Lightbox Preview (Gambar & Video Tanpa Suara) -->

    <Transition

      enter-active-class="transition duration-200 ease-out"

      enter-from-class="opacity-0"

      enter-to-class="opacity-100"

      leave-active-class="transition duration-150 ease-in"

      leave-from-class="opacity-100"

      leave-to-class="opacity-0"

    >

      <div

        v-if="activePreview"

        class="fixed inset-0 z-[100] flex flex-col items-center justify-center p-4 bg-black/95 backdrop-blur-lg"

        @click="closePreview"

      >

        <button

          class="absolute top-4 right-4 sm:top-6 sm:right-6 text-white bg-white/15 hover:bg-white/25 border border-white/20 w-10 h-10 rounded-full flex items-center justify-center transition-colors z-10"

          @click.stop="closePreview"

          aria-label="Tutup pratinjau"

        >

          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" aria-hidden="true">

            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />

          </svg>

        </button>



        <p class="text-sm text-white font-medium mb-3 text-center px-8">

          {{ activePreview.title }}

        </p>



        <img

          v-if="activePreview.type === 'image'"

          :src="activePreview.src"

          :alt="activePreview.title"

          class="w-auto h-auto max-w-[95vw] max-h-[80vh] object-contain rounded-xl border border-white/10 shadow-2xl"

        />



        <!-- Video Preview Berjalan Otomatis Tanpa Suara (Silent Loop) -->

        <video

          v-else

          :src="activePreview.src"

          autoplay

          loop

          muted

          playsinline

          class="w-auto h-auto max-w-[95vw] max-h-[80vh] object-contain rounded-xl border border-white/10 shadow-2xl"

        ></video>



        <p class="text-xs text-gray-300 mt-4 font-sans bg-white/10 px-3 py-1 rounded-full">

          Ketuk di mana saja atau tekan Esc untuk menutup

        </p>

      </div>

    </Transition>

  &lt;/section&gt;

&lt;/template&gt;


