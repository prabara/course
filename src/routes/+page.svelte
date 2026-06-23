<script lang="ts">
  import { slide } from 'svelte/transition';
  import { onMount } from 'svelte';

  // State using Svelte 5 Runes
  let activeTab = $state('speaking');
  let faqOpen = $state(Array(4).fill(false));
  let testimonialIndex = $state(0);
  let mobileMenuOpen = $state(false);
  let formSubmitted = $state(false);
  let formData = $state({ name: '', phone: '', grade: '1', message: '' });

  // WhatsApp configuration
  const whatsappNumber = '6281234567890'; // Replace with real number if needed
  const getWhatsAppLink = (text: string) => `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(text)}`;

  // Dynamic testimonial carousel interval
  onMount(() => {
    const interval = setInterval(() => {
      testimonialIndex = (testimonialIndex + 1) % testimonials.length;
    }, 6000);
    return () => clearInterval(interval);
  });

  const whyChooseUs = [
    {
      title: 'Fun Learning',
      desc: 'Belajar dengan metode interaktif tanpa tekanan, menggunakan nyanyian, aktivitas fisik, dan alat peraga menarik.',
      color: 'blue',
      icon: `<svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M14.828 14.828a4 4 0 01-5.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>`
    },
    {
      title: 'Interactive Games',
      desc: 'Game edukatif yang melatih kefasihan kosakata dan keberanian berbicara sambil berkompetisi secara sehat.',
      color: 'yellow',
      icon: `<svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M11 4a2 2 0 114 0v1a2 2 0 002 2h3a2 2 0 012 2v3a2 2 0 002 2v6a2 2 0 01-2 2h-3a2 2 0 00-2-2v-1a2 2 0 00-4 0v1a2 2 0 01-2 2H2a2 2 0 01-2-2v-6a2 2 0 002-2v-3a2 2 0 012-2h3a2 2 0 002-2V4z"></path></svg>`
    },
    {
      title: 'Qualified Teachers',
      desc: 'Pengajar profesional bersertifikasi yang ramah, enerjik, dan mengerti psikologi perkembangan anak.',
      color: 'orange',
      icon: `<svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M12 14l9-5-9-5-9 5 9 5zm0 0l6.16-3.422a12.083 12.083 0 01.665 6.479A11.952 11.952 0 0012 20.055a11.952 11.952 0 00-6.824-2.998 12.078 12.078 0 01.665-6.479L12 14zm-4 6v-7.5l4-2.222" /> </svg>`
    },
    {
      title: 'Small Class Size',
      desc: 'Jumlah siswa per kelas dibatasi maksimal 8-10 anak agar guru dapat memantau perkembangan setiap anak secara personal.',
      color: 'green',
      icon: `<svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"></path></svg>`
    }
  ];

  const programs = [
    {
      title: 'Junior Stars',
      subtitle: 'Grade 1 - 3 (Ages 6-9)',
      desc: 'Membangun rasa percaya diri anak dengan memperbanyak kosakata dasar lewat aktivitas bermain, bernyanyi, dan menirukan suara.',
      color: 'yellow',
      features: ['Kosakata Dasar Terapan', 'Penyebaran Aksen Alami', 'Lagu & Gerakan Interaktif', 'Buku Kegiatan Bergambar'],
      shadowColor: '#FFC01E',
      badge: 'Paling Populer'
    },
    {
      title: 'Senior Explorer',
      subtitle: 'Grade 4 - 6 (Ages 10-12)',
      desc: 'Mulai mendalami struktur tata bahasa dasar dan membaca cerita pendek serta melatih diskusi kelompok agar anak kritis berbicara.',
      color: 'blue',
      features: ['Percakapan Sehari-hari', 'Pengenalan Grammar Dasar', 'Storytelling & Diskusi', 'Latihan Menulis Kreatif'],
      shadowColor: '#2A82E6',
      badge: 'Rekomendasi'
    },
    {
      title: 'Private Buddy',
      subtitle: 'Personalized Class (Ages 6-12)',
      desc: 'Kelas privat satu-satu (one-on-one) dengan kurikulum yang sepenuhnya disesuaikan dengan kebutuhan belajar spesifik anak.',
      color: 'orange',
      features: ['100% Guru Fokus pada 1 Anak', 'Jadwal Sangat Fleksibel', 'Laporan Progres Sesi per Sesi', 'Materi Custom Sesuai Target'],
      shadowColor: '#FF7034',
      badge: 'Eksklusif'
    }
  ];

  const activities = {
    speaking: {
      title: 'Percakapan Percaya Diri (Speaking)',
      desc: 'Anak-anak dilatih untuk berani melafalkan kata-kata dalam bahasa Inggris sejak hari pertama melalui roleplay dan skenario menyenangkan.',
      image: '/static/hero_kids_learning.png', // Fallback or placeholder
      badge: 'Speaking',
      color: 'orange',
      points: [
        'Simulasi percakapan sehari-hari',
        'Bernyanyi bersama untuk intonasi yang pas',
        'Pementasan drama mini sederhana'
      ]
    },
    vocabulary: {
      title: 'Kosakata Kaya & Kreatif (Vocabulary)',
      desc: 'Belajar kosakata tanpa menghafal yang membosankan! Kami menggunakan flashcards interaktif, objek riil, dan visualisasi menarik.',
      badge: 'Vocabulary',
      color: 'yellow',
      points: [
        'Asosiasi visual gambar-objek',
        'Game ejaan kata (Mini Spelling Bee)',
        'Eksplorasi kata baru melalui alam sekitar'
      ]
    },
    storytelling: {
      title: 'Dunia Dongeng Interaktif (Storytelling)',
      desc: 'Mendengarkan kisah menarik dalam bahasa Inggris meningkatkan pemahaman mendengar (listening comprehension) sekaligus imajinasi kreatif.',
      badge: 'Storytelling',
      color: 'blue',
      points: [
        'Membaca bersama buku bergambar (Big Books)',
        'Dongeng interaktif dengan boneka tangan',
        'Membuat kelanjutan cerita imajinatif sendiri'
      ]
    },
    games: {
      title: 'Bermain Sambil Belajar (Educational Games)',
      desc: 'Setiap sesi diselingi dengan game edukasi yang dirancang untuk mempraktikkan materi pembelajaran secara menyenangkan dan dinamis.',
      badge: 'Games',
      color: 'green',
      points: [
        'Papan permainan (board game) edukasi',
        'Kuis interaktif kelompok kecil',
        'Teka-teki bahasa dan kompetisi seru'
      ]
    }
  };

  const testimonials = [
    {
      name: 'Bunda Rina',
      relation: 'Orang tua Kevin (Grade 3)',
      text: 'Sangat bersyukur menyekolahkan Kevin di Joyfull English. Anak saya yang tadinya pendiam sekarang suka sekali menyanyi dan menyapa dalam bahasa Inggris di rumah!',
      color: 'blue',
      avatar: '👩‍💼'
    },
    {
      name: 'Bunda Shinta',
      relation: 'Orang tua Kiara (Grade 5)',
      text: 'Metode belajarnya seru sekali! Kiara selalu bersemangat setiap kali tiba hari lesnya. Nilai ujian bahasa Inggris di sekolahnya sekarang selalu di atas 90!',
      color: 'yellow',
      avatar: '👩‍⚕'
    },
    {
      name: 'Ayah Budi',
      relation: 'Orang tua Rian (Grade 2)',
      text: 'Gurunya sabar dan pandai menarik perhatian anak kecil. Kelasnya sangat representatif, ber-AC, dan bersih. Anak tidak merasa terbebani seperti sekolah formal.',
      color: 'orange',
      avatar: '👨‍💼'
    }
  ];

  const faqs = [
    {
      q: 'Apakah anak bisa mengikuti kelas coba gratis (Free Trial)?',
      a: 'Tentu saja! Kami menyediakan 1x sesi Trial Gratis secara langsung agar anak Anda bisa merasakan keseruan belajar dengan metode interaktif kami sebelum Anda memutuskan mendaftar.'
    },
    {
      q: 'Berapa banyak anak dalam satu kelas?',
      a: 'Untuk menjaga kualitas belajar mengajar, kelas reguler kami dibatasi maksimal 8 hingga 10 anak saja. Hal ini memastikan guru memiliki waktu membimbing anak Anda secara personal.'
    },
    {
      q: 'Bagaimana jika anak belum pernah belajar bahasa Inggris?',
      a: 'Tidak masalah! Program belajar kami didesain bertahap mulai dari tingkat dasar dengan pengenalan visual, gestures, dan nyanyian ramah pemula, sehingga anak akan beradaptasi dengan sangat cepat.'
    },
    {
      q: 'Bagaimana sistem pembayaran dan pilihan jadwalnya?',
      a: 'Kami menawarkan pembayaran bulanan yang terjangkau. Pilihan hari belajar reguler adalah Senin-Rabu atau Selasa-Kamis sore (15.30 - 17.00). Untuk kelas Private, jadwal dapat didiskusikan fleksibel.'
    }
  ];

  function toggleFaq(index: number) {
    faqOpen = faqOpen.map((val, idx) => idx === index ? !val : val);
  }

  function handleFormSubmit(e: Event) {
    e.preventDefault();
    formSubmitted = true;
    
    // Auto redirect to WhatsApp with filled details
    const text = `Halo Joyfull English!\n\nSaya ingin mendaftarkan anak saya untuk kelas gratis (Free Trial):\n- Nama Orang Tua: ${formData.name}\n- WhatsApp/No HP: ${formData.phone}\n- Kelas Anak: Grade ${formData.grade}\n- Pesan Tambahan: ${formData.message || '-'}`;
    
    setTimeout(() => {
      window.open(getWhatsAppLink(text), '_blank');
      formSubmitted = false;
      formData = { name: '', phone: '', grade: '1', message: '' };
    }, 1500);
  }

  function toggleMobileMenu() {
    mobileMenuOpen = !mobileMenuOpen;
  }
</script>

<!-- Header & Navigation -->
<header class="navbar">
  <div class="container nav-container">
    <a href="#hero" class="logo" aria-label="Joyfull English Home">
      <span class="logo-emoji">🎈</span>
      <span class="logo-text">Joyfull<span class="logo-accent">English</span></span>
    </a>

    <!-- Desktop Navigation Links -->
    <nav class="nav-links">
      <a href="#why-choose" class="nav-item">Kenapa Kami</a>
      <a href="#programs" class="nav-item">Program</a>
      <a href="#activities" class="nav-item">Aktivitas</a>
      <a href="#testimonials" class="nav-item">Testimoni</a>
      <a href="#faq" class="nav-item">FAQ</a>
      <a href="#contact" class="btn-nav">Hubungi Kami</a>
    </nav>

    <!-- Mobile Menu Button -->
    <button class="mobile-toggle" aria-label="Toggle Menu" onclick={toggleMobileMenu}>
      <div class="hamburger-bar {mobileMenuOpen ? 'open' : ''}"></div>
    </button>
  </div>

  <!-- Mobile Dropdown Navigation -->
  {#if mobileMenuOpen}
    <div class="mobile-nav" transition:slide={{ duration: 300 }}>
      <a href="#why-choose" onclick={toggleMobileMenu}>Kenapa Kami</a>
      <a href="#programs" onclick={toggleMobileMenu}>Program</a>
      <a href="#activities" onclick={toggleMobileMenu}>Aktivitas</a>
      <a href="#testimonials" onclick={toggleMobileMenu}>Testimoni</a>
      <a href="#faq" onclick={toggleMobileMenu}>FAQ</a>
      <a href="#contact" class="btn-mobile-contact" onclick={toggleMobileMenu}>Daftar Sekarang</a>
    </div>
  {/if}
</header>

<!-- Hero Section -->
<section id="hero" class="hero-section">
  <!-- Playful Decorative SVGs Background -->
  <div class="bg-shape shape-blob-1"></div>
  <div class="bg-shape shape-blob-2"></div>
  <div class="bg-shape shape-dots"></div>

  <div class="container hero-grid">
    <div class="hero-content">
      <span class="badge badge-yellow wave-anim">🌈 FUN & INTERACTIVE LEARNING</span>
      <h1 class="hero-title">Belajar Bahasa Inggris Jadi <span class="text-orange">Menyenangkan!</span></h1>
      <p class="hero-desc">
        Program Bahasa Inggris interaktif untuk anak sekolah dasar (usia 6-12 tahun). Belajar secara alami, percaya diri, tanpa rasa tertekan dengan guru ramah & asyik!
      </p>
      
      <div class="hero-actions">
        <a href={getWhatsAppLink('Halo Joyfull English! Saya ingin mendaftar kelas Free Trial untuk anak saya.')} target="_blank" rel="noopener noreferrer" class="btn-primary">
          Daftar Sekarang
          <svg class="icon-arrow" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M14 5l7 7m0 0l-7 7m7-7H3"></path>
          </svg>
        </a>
        <a href="#programs" class="btn-secondary">Lihat Program</a>
      </div>

      <div class="hero-stats">
        <div class="stat-item">
          <span class="stat-num">100%</span>
          <span class="stat-label">Ramah Anak</span>
        </div>
        <div class="stat-item">
          <span class="stat-num">8-10</span>
          <span class="stat-label">Siswa / Kelas</span>
        </div>
        <div class="stat-item">
          <span class="stat-num">1x</span>
          <span class="stat-label">Trial Gratis</span>
        </div>
      </div>
    </div>
    
    <div class="hero-image-container float-anim">
      <div class="image-frame-bg"></div>
      <img src="/hero_kids_learning.png" alt="Happy Kids Learning English" class="hero-img" />
    </div>
  </div>
</section>

<!-- Why Choose Us Section -->
<section id="why-choose" class="why-choose-section">
  <div class="container">
    <div class="section-header">
      <span class="badge badge-blue">KEUNGGULAN KAMI</span>
      <h2 class="section-title">Mengapa Memilih Joyfull English?</h2>
      <p class="section-subtitle">Kami memahami cara terbaik anak belajar bahasa baru: dengan rasa gembira!</p>
    </div>

    <div class="why-choose-grid">
      {#each whyChooseUs as item}
        <div class="playful-card why-card" style="--shadow-color: var(--color-{item.color}); --border-color: var(--color-text-title);">
          <div class="card-icon icon-{item.color}">
            {@html item.icon}
          </div>
          <h3 class="card-title">{item.title}</h3>
          <p class="card-desc">{item.desc}</p>
        </div>
      {/each}
    </div>
  </div>
</section>

<!-- Programs Section -->
<section id="programs" class="programs-section">
  <div class="container">
    <div class="section-header">
      <span class="badge badge-orange">PROGRAM KELAS</span>
      <h2 class="section-title">Pilih Kelas Terbaik Untuk Buah Hati</h2>
      <p class="section-subtitle">Program berjenjang yang disesuaikan dengan tingkat usia dan tumbuh kembang anak.</p>
    </div>

    <div class="programs-grid">
      {#each programs as prog}
        <div class="program-card playful-card" style="--shadow-color: {prog.shadowColor}; --border-color: var(--color-text-title);">
          <div class="prog-badge">{prog.badge}</div>
          <h3 class="prog-title">{prog.title}</h3>
          <p class="prog-subtitle">{prog.subtitle}</p>
          <div class="prog-divider"></div>
          <p class="prog-desc">{prog.desc}</p>
          
          <ul class="prog-features">
            {#each prog.features as feature}
              <li>
                <svg class="w-5 h-5 text-green" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M5 13l4 4L19 7"></path></svg>
                <span>{feature}</span>
              </li>
            {/each}
          </ul>
          
          <a href={getWhatsAppLink(`Halo Joyfull English! Saya ingin bertanya tentang Program ${prog.title}`)} target="_blank" rel="noopener noreferrer" class="btn-program-cta btn-primary" style="background-color: {prog.shadowColor}; color: var(--color-text-title);">
            Tanya Program
          </a>
        </div>
      {/each}
    </div>
  </div>
</section>

<!-- Learning Activities Section -->
<section id="activities" class="activities-section">
  <div class="container">
    <div class="section-header">
      <span class="badge badge-blue">METODE BELAJAR</span>
      <h2 class="section-title">Aktivitas Seru di Setiap Sesi</h2>
      <p class="section-subtitle">Kami menolak kelas pasif. Di sini, setiap anak ikut bergerak dan berkontribusi.</p>
    </div>

    <!-- Interactive Svelte Tabs -->
    <div class="tabs-container">
      <div class="tabs-buttons">
        <button class="tab-btn {activeTab === 'speaking' ? 'active-speaking' : ''}" onclick={() => activeTab = 'speaking'}>
          🎤 Speaking
        </button>
        <button class="tab-btn {activeTab === 'vocabulary' ? 'active-vocabulary' : ''}" onclick={() => activeTab = 'vocabulary'}>
          📚 Vocabulary
        </button>
        <button class="tab-btn {activeTab === 'storytelling' ? 'active-storytelling' : ''}" onclick={() => activeTab = 'storytelling'}>
          ✨ Storytelling
        </button>
        <button class="tab-btn {activeTab === 'games' ? 'active-games' : ''}" onclick={() => activeTab = 'games'}>
          🎮 Educational Games
        </button>
      </div>

      <!-- Tab Content Area -->
      <div class="tab-content-wrapper playful-card" style="--shadow-color: var(--color-blue)">
        {#each Object.entries(activities) as [key, value]}
          {#if activeTab === key}
            <div class="tab-content" transition:slide={{ duration: 400 }}>
              <div class="tab-text-side">
                <span class="badge badge-{value.color}">{value.badge} Focus</span>
                <h3 class="tab-content-title">{value.title}</h3>
                <p class="tab-content-desc">{value.desc}</p>
                <div class="tab-divider"></div>
                <h4 class="points-header">Apa yang dipelajari?</h4>
                <ul class="tab-points">
                  {#each value.points as pt}
                    <li>
                      <div class="point-bullet bullet-{value.color}"></div>
                      <span>{pt}</span>
                    </li>
                  {/each}
                </ul>
              </div>
              <div class="tab-graphic-side">
                <!-- Clean themed illustration frame -->
                <div class="theme-frame frame-{value.color}">
                  <span class="decorative-emoji float-anim">🎈</span>
                  <span class="decorative-emoji-2 wave-anim">🌟</span>
                  <div class="graphic-circle circle-{value.color}"></div>
                  <div class="graphic-text">Joyfull!</div>
                </div>
              </div>
            </div>
          {/if}
        {/each}
      </div>
    </div>
  </div>
</section>

<!-- Parent Testimonials Section -->
<section id="testimonials" class="testimonials-section">
  <div class="container">
    <div class="section-header">
      <span class="badge badge-yellow">KATA ORANG TUA</span>
      <h2 class="section-title">Apa Kata Ayah & Bunda?</h2>
      <p class="section-subtitle">Kisah nyata dari para orang tua yang melihat langsung perkembangan anak-anaknya.</p>
    </div>

    <!-- Testimonials Carousel -->
    <div class="carousel-container">
      <div class="carousel-track">
        {#each testimonials as testimonial, idx}
          {#if idx === testimonialIndex}
            <div class="testimonial-card playful-card" transition:slide={{ duration: 400 }} style="--shadow-color: var(--color-{testimonial.color});">
              <div class="quote-icon">“</div>
              <p class="test-text">{testimonial.text}</p>
              <div class="test-user">
                <div class="test-avatar">{testimonial.avatar}</div>
                <div class="test-meta">
                  <h4 class="test-name">{testimonial.name}</h4>
                  <p class="test-relation">{testimonial.relation}</p>
                </div>
              </div>
            </div>
          {/if}
        {/each}
      </div>

      <!-- Controls -->
      <div class="carousel-controls">
        <button class="control-btn" aria-label="Previous Testimonial" onclick={() => testimonialIndex = (testimonialIndex - 1 + testimonials.length) % testimonials.length}>
          ←
        </button>
        <div class="carousel-dots">
          {#each testimonials as _, idx}
            <button class="dot {idx === testimonialIndex ? 'active' : ''}" aria-label="Go to testimonial {idx+1}" onclick={() => testimonialIndex = idx}></button>
          {/each}
        </div>
        <button class="control-btn" aria-label="Next Testimonial" onclick={() => testimonialIndex = (testimonialIndex + 1) % testimonials.length}>
          →
        </button>
      </div>
    </div>
  </div>
</section>

<!-- FAQ Section -->
<section id="faq" class="faq-section">
  <div class="container faq-container">
    <div class="section-header">
      <span class="badge badge-orange">TANYA JAWAB</span>
      <h2 class="section-title">Pertanyaan Sering Diajukan</h2>
      <p class="section-subtitle">Temukan jawaban cepat seputar proses belajar di Joyfull English.</p>
    </div>

    <div class="faq-list">
      {#each faqs as faq, idx}
        <div class="faq-item playful-card {faqOpen[idx] ? 'faq-expanded' : ''}" style="--shadow-color: #E2E8F0; padding: 20px;">
          <button class="faq-question" onclick={() => toggleFaq(idx)}>
            <span>{faq.q}</span>
            <span class="faq-toggle-icon">{faqOpen[idx] ? '▲' : '▼'}</span>
          </button>
          {#if faqOpen[idx]}
            <div class="faq-answer" transition:slide={{ duration: 250 }}>
              <p>{faq.a}</p>
            </div>
          {/if}
        </div>
      {/each}
    </div>
  </div>
</section>

<!-- Contact Section & Form -->
<section id="contact" class="contact-section">
  <div class="container contact-grid">
    <div class="contact-info-side">
      <span class="badge badge-blue">DAFTAR KELAS</span>
      <h2 class="contact-title">Mulai Petualangan Seru Sekarang!</h2>
      <p class="contact-desc">
        Hubungi kami atau isi formulir untuk menjadwalkan **Free Trial Class**. Staff kami akan menyambut Anda dengan hangat.
      </p>

      <div class="contact-details">
        <div class="detail-card playful-card" style="--shadow-color: var(--color-blue); padding: 20px; display: flex; gap: 15px; align-items: flex-start;">
          <div class="detail-icon" style="color: var(--color-blue)">📍</div>
          <div>
            <h4>Alamat Center</h4>
            <p>Jl. Pendidikan No. 45, Kebayoran Baru, Jakarta Selatan, 12130</p>
          </div>
        </div>

        <div class="detail-card playful-card" style="--shadow-color: var(--color-yellow); padding: 20px; display: flex; gap: 15px; align-items: flex-start;">
          <div class="detail-icon" style="color: var(--color-yellow-dark)">📞</div>
          <div>
            <h4>Telepon</h4>
            <p>+62 21-5556-7890</p>
          </div>
        </div>

        <div class="detail-card playful-card" style="--shadow-color: var(--color-orange); padding: 20px; display: flex; gap: 15px; align-items: flex-start;">
          <div class="detail-icon" style="color: var(--color-orange)">💬</div>
          <div>
            <h4>WhatsApp Official</h4>
            <p>+62 812-3456-7890</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Interactive Registration Form -->
    <div class="contact-form-side">
      <div class="form-container playful-card" style="--shadow-color: var(--color-orange)">
        {#if formSubmitted}
          <div class="success-message" transition:slide>
            <div class="success-icon">🎉</div>
            <h3>Pendaftaran Terkirim!</h3>
            <p>Terima kasih! Kami akan mengalihkan Anda ke WhatsApp Official kami untuk konfirmasi jadwal kelas coba gratis Anda...</p>
          </div>
        {:else}
          <h3 class="form-header-title">Daftar Free Trial Class 🎁</h3>
          <form onsubmit={handleFormSubmit} class="registration-form">
            <div class="form-group">
              <label for="parent-name">Nama Orang Tua</label>
              <input type="text" id="parent-name" bind:value={formData.name} placeholder="Contoh: Bunda Rina" required />
            </div>

            <div class="form-group">
              <label for="parent-phone">Nomor WhatsApp</label>
              <input type="tel" id="parent-phone" bind:value={formData.phone} placeholder="Contoh: 0812XXXXXXXX" required />
            </div>

            <div class="form-group">
              <label for="child-grade">Kelas Anak saat ini</label>
              <select id="child-grade" bind:value={formData.grade}>
                <option value="1">Grade 1 SD (Umur 6-7)</option>
                <option value="2">Grade 2 SD (Umur 7-8)</option>
                <option value="3">Grade 3 SD (Umur 8-9)</option>
                <option value="4">Grade 4 SD (Umur 9-10)</option>
                <option value="5">Grade 5 SD (Umur 10-11)</option>
                <option value="6">Grade 6 SD (Umur 11-12)</option>
              </select>
            </div>

            <div class="form-group">
              <label for="notes">Pesan Tambahan (Opsional)</label>
              <textarea id="notes" bind:value={formData.message} placeholder="Tulis pertanyaan Anda di sini..." rows="3"></textarea>
            </div>

            <button type="submit" class="btn-whatsapp w-full">
              Kirim via WhatsApp
            </button>
          </form>
        {/if}
      </div>
    </div>
  </div>
</section>

<!-- Floating WhatsApp Button -->
<a 
  href={getWhatsAppLink('Halo Joyfull English! Saya ingin bertanya lebih lanjut tentang program bimbingan bahasa Inggris.')} 
  target="_blank" 
  rel="noopener noreferrer" 
  class="floating-whatsapp pulse-anim"
  aria-label="Chat WhatsApp"
>
  <svg class="w-8 h-8" fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
    <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L0 24l6.335-1.662c1.746.953 3.71 1.458 5.704 1.459h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413Z"/>
  </svg>
</a>

<!-- Footer -->
<footer class="footer">
  <div class="container footer-content">
    <div class="footer-brand">
      <a href="#hero" class="logo" aria-label="Joyfull English Home">
        <span class="logo-emoji">🎈</span>
        <span class="logo-text">Joyfull<span class="logo-accent">English</span></span>
      </a>
      <p class="footer-motto">Belajar Bahasa Inggris Jadi Menyenangkan!</p>
    </div>
    
    <div class="footer-links">
      <h4>Navigasi</h4>
      <a href="#why-choose">Kenapa Kami</a>
      <a href="#programs">Program Kelas</a>
      <a href="#activities">Aktivitas</a>
      <a href="#testimonials">Testimoni</a>
      <a href="#faq">FAQ</a>
    </div>

    <div class="footer-socials">
      <h4>Ikuti Kami</h4>
      <div class="social-icons">
        <a href="https://instagram.com" target="_blank" rel="noopener noreferrer" class="social-icon" aria-label="Instagram">📸</a>
        <a href="https://facebook.com" target="_blank" rel="noopener noreferrer" class="social-icon" aria-label="Facebook">🔵</a>
        <a href="https://youtube.com" target="_blank" rel="noopener noreferrer" class="social-icon" aria-label="YouTube">🔴</a>
      </div>
      <p class="footer-contact-text">Email: info@joyfullenglish.com</p>
    </div>
  </div>
  
  <div class="footer-bottom">
    <p>&copy; {new Date().getFullYear()} Joyfull English. All Rights Reserved. Made with ❤️ for happy learning.</p>
  </div>
</footer>

<style>
  /* Local styles scoped to the Landing Page */

  /* Navbar Styles */
  .navbar {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 1000;
    background-color: rgba(255, 255, 255, 0.9);
    backdrop-filter: blur(10px);
    border-bottom: 3px solid var(--color-text-title);
    padding: 15px 0;
  }

  .nav-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .logo {
    display: inline-flex;
    align-items: center;
    gap: 10px;
  }

  .logo-emoji {
    font-size: 2rem;
  }

  .logo-text {
    font-family: var(--font-header);
    font-size: 1.8rem;
    font-weight: 700;
    color: var(--color-blue);
  }

  .logo-accent {
    color: var(--color-orange);
  }

  .nav-links {
    display: none;
    align-items: center;
    gap: 25px;
  }

  @media (min-width: 992px) {
    .nav-links {
      display: flex;
    }
  }

  .nav-item {
    font-family: var(--font-header);
    font-weight: 600;
    font-size: 1.05rem;
    color: var(--color-text-title);
    padding: 6px 12px;
    border-radius: 100px;
  }

  .nav-item:hover {
    color: var(--color-blue);
    background-color: var(--color-blue-light);
  }

  .btn-nav {
    background-color: var(--color-yellow);
    color: var(--color-text-title);
    padding: 10px 20px;
    border-radius: var(--radius-sm);
    font-size: 1rem;
    border: 3px solid var(--color-text-title);
    box-shadow: 3px 3px 0px var(--color-text-title);
  }

  .btn-nav:hover {
    transform: translate(-2px, -2px);
    box-shadow: 5px 5px 0px var(--color-text-title);
    background-color: #ffd051;
  }

  .mobile-toggle {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 44px;
    height: 44px;
    border: 3px solid var(--color-text-title);
    background-color: var(--color-yellow-light);
    border-radius: 10px;
  }

  @media (min-width: 992px) {
    .mobile-toggle {
      display: none;
    }
  }

  .hamburger-bar {
    width: 20px;
    height: 3px;
    background-color: var(--color-text-title);
    position: relative;
    transition: var(--transition-fast);
  }

  .hamburger-bar::before,
  .hamburger-bar::after {
    content: '';
    position: absolute;
    width: 20px;
    height: 3px;
    background-color: var(--color-text-title);
    transition: var(--transition-fast);
  }

  .hamburger-bar::before {
    top: -8px;
  }

  .hamburger-bar::after {
    bottom: -8px;
  }

  .hamburger-bar.open {
    background-color: transparent;
  }

  .hamburger-bar.open::before {
    transform: rotate(45deg);
    top: 0;
  }

  .hamburger-bar.open::after {
    transform: rotate(-45deg);
    bottom: 0;
  }

  .mobile-nav {
    display: flex;
    flex-direction: column;
    background-color: var(--color-bg-white);
    border-bottom: 3px solid var(--color-text-title);
    padding: 20px;
    gap: 15px;
  }

  .mobile-nav a {
    font-family: var(--font-header);
    font-size: 1.2rem;
    font-weight: 600;
    color: var(--color-text-title);
    padding: 8px 12px;
    border-radius: 10px;
  }

  .mobile-nav a:hover {
    background-color: var(--color-blue-light);
  }

  .btn-mobile-contact {
    background-color: var(--color-orange) !important;
    color: white !important;
    text-align: center;
    border: 3px solid var(--color-text-title);
    box-shadow: 4px 4px 0px var(--color-text-title);
  }

  /* Hero Section Styles */
  .hero-section {
    padding: 140px 0 80px 0;
    background: var(--color-bg-gradient);
    position: relative;
    overflow: hidden;
  }

  /* Blob SVG decoration */
  .bg-shape {
    position: absolute;
    z-index: 1;
    pointer-events: none;
    border-radius: 50%;
  }

  .shape-blob-1 {
    width: 500px;
    height: 500px;
    background-color: rgba(42, 130, 230, 0.08);
    top: -100px;
    right: -100px;
    filter: blur(40px);
  }

  .shape-blob-2 {
    width: 400px;
    height: 400px;
    background-color: rgba(255, 112, 52, 0.08);
    bottom: -50px;
    left: -100px;
    filter: blur(40px);
  }

  .shape-dots {
    width: 100px;
    height: 100px;
    background-image: radial-gradient(var(--color-text-title) 2px, transparent 2px);
    background-size: 16px 16px;
    top: 150px;
    left: 5%;
    opacity: 0.15;
  }

  .hero-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 40px;
    align-items: center;
    position: relative;
    z-index: 2;
  }

  @media (min-width: 992px) {
    .hero-grid {
      grid-template-columns: 1.2fr 1fr;
    }
  }

  .hero-content {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  .hero-title {
    font-size: 2.8rem;
    margin-top: 15px;
    margin-bottom: 20px;
    color: var(--color-text-title);
  }

  @media (min-width: 768px) {
    .hero-title {
      font-size: 3.8rem;
    }
  }

  .text-orange {
    color: var(--color-orange);
  }

  .hero-desc {
    font-size: 1.15rem;
    color: var(--color-text-body);
    margin-bottom: 30px;
    max-width: 580px;
  }

  .hero-actions {
    display: flex;
    flex-wrap: wrap;
    gap: 15px;
    margin-bottom: 40px;
  }

  .icon-arrow {
    width: 20px;
    height: 20px;
    margin-left: 10px;
    transition: transform var(--transition-fast);
  }

  .btn-primary:hover .icon-arrow {
    transform: translateX(4px);
  }

  .hero-stats {
    display: flex;
    gap: 30px;
  }

  .stat-item {
    display: flex;
    flex-direction: column;
  }

  .stat-num {
    font-family: var(--font-header);
    font-size: 2.2rem;
    font-weight: 700;
    color: var(--color-blue);
    line-height: 1;
  }

  .stat-label {
    font-size: 0.9rem;
    font-weight: 600;
    color: var(--color-text-body);
  }

  .hero-image-container {
    display: flex;
    justify-content: center;
    position: relative;
  }

  .image-frame-bg {
    position: absolute;
    width: 90%;
    height: 90%;
    background-color: var(--color-yellow);
    border: 3px solid var(--color-text-title);
    border-radius: var(--radius-lg);
    z-index: 1;
    transform: rotate(3deg);
    top: 5%;
    left: 5%;
  }

  .hero-img {
    max-width: 100%;
    height: auto;
    border-radius: var(--radius-lg);
    border: 3px solid var(--color-text-title);
    z-index: 2;
    background-color: white;
  }

  /* Section Header */
  .section-header {
    text-align: center;
    max-width: 700px;
    margin: 0 auto 50px auto;
  }

  .section-title {
    font-size: 2.2rem;
    margin-top: 15px;
    margin-bottom: 15px;
    color: var(--color-text-title);
  }

  @media (min-width: 768px) {
    .section-title {
      font-size: 2.8rem;
    }
  }

  .section-subtitle {
    font-size: 1.1rem;
    color: var(--color-text-body);
  }

  /* Why Choose Us Section */
  .why-choose-section {
    padding: 80px 0;
    background-color: var(--color-bg-cream);
  }

  .why-choose-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 30px;
  }

  @media (min-width: 576px) {
    .why-choose-grid {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  @media (min-width: 992px) {
    .why-choose-grid {
      grid-template-columns: repeat(4, 1fr);
    }
  }

  .why-card {
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .card-icon {
    display: inline-flex;
    padding: 15px;
    border-radius: 50%;
    margin-bottom: 20px;
    border: 3px solid var(--color-text-title);
  }

  .icon-blue { background-color: var(--color-blue-light); color: var(--color-blue); }
  .icon-yellow { background-color: var(--color-yellow-light); color: var(--color-yellow-dark); }
  .icon-orange { background-color: var(--color-orange-light); color: var(--color-orange); }
  .icon-green { background-color: var(--color-green-light); color: var(--color-green-dark); }

  .card-title {
    font-size: 1.4rem;
    margin-bottom: 12px;
  }

  .card-desc {
    font-size: 0.95rem;
    color: var(--color-text-body);
  }

  /* Programs Section */
  .programs-section {
    padding: 80px 0;
  }

  .programs-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 35px;
  }

  @media (min-width: 768px) {
    .programs-grid {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  @media (min-width: 992px) {
    .programs-grid {
      grid-template-columns: repeat(3, 1fr);
    }
  }

  .program-card {
    display: flex;
    flex-direction: column;
    position: relative;
    padding-top: 50px;
    overflow: hidden;
  }

  .prog-badge {
    position: absolute;
    top: 15px;
    right: 15px;
    background-color: var(--color-text-title);
    color: white;
    font-family: var(--font-header);
    font-size: 0.8rem;
    padding: 4px 12px;
    border-radius: 100px;
  }

  .prog-title {
    font-size: 1.8rem;
    margin-bottom: 5px;
  }

  .prog-subtitle {
    font-weight: 700;
    color: var(--color-text-body);
    font-size: 0.95rem;
    margin-bottom: 15px;
  }

  .prog-divider {
    height: 3px;
    background-color: var(--color-text-title);
    margin-bottom: 20px;
    width: 40px;
  }

  .prog-desc {
    font-size: 0.95rem;
    color: var(--color-text-body);
    margin-bottom: 25px;
    flex-grow: 1;
  }

  .prog-features {
    list-style: none;
    margin-bottom: 30px;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .prog-features li {
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 0.95rem;
    font-weight: 600;
  }

  .text-green {
    color: var(--color-green);
  }

  .btn-program-cta {
    width: 100%;
    margin-top: auto;
    text-align: center;
    border: 3px solid var(--color-text-title);
  }

  /* Activities Section */
  .activities-section {
    padding: 80px 0;
    background-color: var(--color-yellow-light);
  }

  .tabs-container {
    width: 100%;
  }

  .tabs-buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    margin-bottom: 25px;
    justify-content: center;
  }

  .tab-btn {
    font-family: var(--font-header);
    font-size: 1.1rem;
    font-weight: 700;
    padding: 12px 24px;
    background-color: white;
    border: 3px solid var(--color-text-title);
    border-radius: var(--radius-sm);
    box-shadow: 4px 4px 0px var(--color-text-title);
  }

  .tab-btn:hover {
    transform: translate(-2px, -2px);
    box-shadow: 6px 6px 0px var(--color-text-title);
  }

  .tab-btn:active {
    transform: translate(1px, 1px);
    box-shadow: 2px 2px 0px var(--color-text-title);
  }

  .active-speaking { background-color: var(--color-orange); color: white; }
  .active-vocabulary { background-color: var(--color-yellow); color: var(--color-text-title); }
  .active-storytelling { background-color: var(--color-blue); color: white; }
  .active-games { background-color: var(--color-green); color: white; }

  .tab-content-wrapper {
    background-color: white;
  }

  .tab-content {
    display: grid;
    grid-template-columns: 1fr;
    gap: 40px;
    align-items: center;
  }

  @media (min-width: 768px) {
    .tab-content {
      grid-template-columns: 1.2fr 0.8fr;
    }
  }

  .tab-text-side {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  .tab-content-title {
    font-size: 2rem;
    margin-top: 15px;
    margin-bottom: 15px;
  }

  .tab-content-desc {
    color: var(--color-text-body);
    font-size: 1.05rem;
    margin-bottom: 20px;
  }

  .tab-divider {
    width: 60px;
    height: 4px;
    background-color: var(--color-text-title);
    margin-bottom: 20px;
  }

  .points-header {
    font-size: 1.1rem;
    margin-bottom: 12px;
  }

  .tab-points {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .tab-points li {
    display: flex;
    align-items: center;
    gap: 15px;
    font-weight: 600;
  }

  .point-bullet {
    width: 14px;
    height: 14px;
    border-radius: 50%;
    border: 3px solid var(--color-text-title);
  }

  .bullet-orange { background-color: var(--color-orange); }
  .bullet-yellow { background-color: var(--color-yellow); }
  .bullet-blue { background-color: var(--color-blue); }
  .bullet-green { background-color: var(--color-green); }

  .tab-graphic-side {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  /* Playful Graphic Box */
  .theme-frame {
    position: relative;
    width: 250px;
    height: 250px;
    border-radius: var(--radius-lg);
    border: 4px dashed var(--color-text-title);
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .frame-orange { background-color: var(--color-orange-light); }
  .frame-yellow { background-color: var(--color-yellow-light); }
  .frame-blue { background-color: var(--color-blue-light); }
  .frame-green { background-color: var(--color-green-light); }

  .decorative-emoji {
    position: absolute;
    top: -20px;
    left: -20px;
    font-size: 3rem;
  }

  .decorative-emoji-2 {
    position: absolute;
    bottom: -20px;
    right: -20px;
    font-size: 3rem;
  }

  .graphic-circle {
    width: 140px;
    height: 140px;
    border-radius: 50%;
    border: 3px solid var(--color-text-title);
  }

  .circle-orange { background-color: var(--color-orange); }
  .circle-yellow { background-color: var(--color-yellow); }
  .circle-blue { background-color: var(--color-blue); }
  .circle-green { background-color: var(--color-green); }

  .graphic-text {
    position: absolute;
    font-family: var(--font-header);
    font-size: 2rem;
    font-weight: 700;
    color: white;
    text-shadow: 2px 2px 0px var(--color-text-title);
    transform: rotate(-10deg);
  }

  /* Testimonials Section */
  .testimonials-section {
    padding: 80px 0;
  }

  .carousel-container {
    max-width: 800px;
    margin: 0 auto;
  }

  .carousel-track {
    min-height: 250px;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .testimonial-card {
    position: relative;
    padding: 40px;
  }

  .quote-icon {
    position: absolute;
    top: -20px;
    left: 20px;
    font-size: 6rem;
    font-family: var(--font-header);
    opacity: 0.15;
    line-height: 1;
  }

  .test-text {
    font-size: 1.15rem;
    font-weight: 500;
    color: var(--color-text-title);
    margin-bottom: 25px;
    line-height: 1.8;
  }

  .test-user {
    display: flex;
    align-items: center;
    gap: 15px;
  }

  .test-avatar {
    width: 50px;
    height: 50px;
    background-color: var(--color-bg-cream);
    border: 2px solid var(--color-text-title);
    border-radius: 50%;
    font-size: 1.8rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .test-name {
    font-size: 1.1rem;
    line-height: 1.2;
  }

  .test-relation {
    font-size: 0.9rem;
    color: var(--color-text-body);
  }

  .carousel-controls {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 20px;
    margin-top: 30px;
  }

  .control-btn {
    width: 44px;
    height: 44px;
    border: 3px solid var(--color-text-title);
    border-radius: 50%;
    font-size: 1.2rem;
    font-weight: 700;
    background-color: white;
    box-shadow: 3px 3px 0px var(--color-text-title);
  }

  .control-btn:hover {
    transform: translate(-2px, -2px);
    box-shadow: 5px 5px 0px var(--color-text-title);
  }

  .control-btn:active {
    transform: translate(1px, 1px);
    box-shadow: 1px 1px 0px var(--color-text-title);
  }

  .carousel-dots {
    display: flex;
    gap: 8px;
  }

  .dot {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    border: 2px solid var(--color-text-title);
    background-color: white;
    cursor: pointer;
    transition: var(--transition-fast);
  }

  .dot.active {
    background-color: var(--color-blue);
    width: 24px;
    border-radius: 100px;
  }

  /* FAQ Section */
  .faq-section {
    padding: 80px 0;
    background-color: var(--color-bg-cream);
  }

  .faq-container {
    max-width: 800px;
  }

  .faq-list {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .faq-item {
    transition: var(--transition-bounce);
  }

  .faq-expanded {
    border-color: var(--color-blue);
    box-shadow: 6px 6px 0px var(--color-blue) !important;
  }

  .faq-question {
    width: 100%;
    text-align: left;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-family: var(--font-header);
    font-size: 1.15rem;
    font-weight: 700;
    color: var(--color-text-title);
    gap: 15px;
  }

  .faq-toggle-icon {
    font-size: 0.9rem;
    color: var(--color-blue);
  }

  .faq-answer {
    margin-top: 15px;
    padding-top: 15px;
    border-top: 2px dashed #E2E8F0;
    color: var(--color-text-body);
    font-size: 1rem;
    line-height: 1.7;
  }

  /* Contact Section */
  .contact-section {
    padding: 80px 0;
    background: linear-gradient(180deg, var(--color-bg-white) 0%, var(--color-blue-light) 100%);
  }

  .contact-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 50px;
  }

  @media (min-width: 992px) {
    .contact-grid {
      grid-template-columns: 1fr 1fr;
    }
  }

  .contact-title {
    font-size: 2.2rem;
    margin-top: 15px;
    margin-bottom: 20px;
  }

  @media (min-width: 768px) {
    .contact-title {
      font-size: 2.8rem;
    }
  }

  .contact-desc {
    font-size: 1.1rem;
    margin-bottom: 40px;
  }

  .contact-details {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .detail-icon {
    font-size: 2rem;
  }

  .contact-form-side {
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .form-container {
    background-color: white;
  }

  .form-header-title {
    font-size: 1.6rem;
    margin-bottom: 25px;
    text-align: center;
  }

  .registration-form {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .form-group {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .form-group label {
    font-family: var(--font-header);
    font-weight: 700;
    font-size: 1rem;
    color: var(--color-text-title);
  }

  .form-group input,
  .form-group select,
  .form-group textarea {
    padding: 12px 18px;
    border: 3px solid var(--color-text-title);
    border-radius: var(--radius-sm);
    font-family: var(--font-body);
    font-size: 1rem;
    color: var(--color-text-title);
    outline: none;
    transition: var(--transition-fast);
  }

  .form-group input:focus,
  .form-group select:focus,
  .form-group textarea:focus {
    border-color: var(--color-blue);
    box-shadow: 4px 4px 0px var(--color-blue-light);
  }

  .w-full {
    width: 100%;
  }

  .success-message {
    text-align: center;
    padding: 40px 10px;
  }

  .success-icon {
    font-size: 4rem;
    margin-bottom: 20px;
  }

  .success-message h3 {
    font-size: 1.8rem;
    margin-bottom: 10px;
    color: var(--color-green-dark);
  }

  .success-message p {
    color: var(--color-text-body);
  }

  /* Floating WhatsApp Button */
  .floating-whatsapp {
    position: fixed;
    bottom: 30px;
    right: 30px;
    z-index: 999;
    background-color: var(--color-green);
    color: white;
    width: 60px;
    height: 60px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 3px solid var(--color-text-title);
    box-shadow: 4px 4px 0px var(--color-text-title);
    transition: var(--transition-bounce);
  }

  .floating-whatsapp:hover {
    transform: translate(-3px, -3px) scale(1.05);
    box-shadow: 7px 7px 0px var(--color-text-title);
    background-color: #2ee073;
  }

  .floating-whatsapp:active {
    transform: translate(1px, 1px);
    box-shadow: 1px 1px 0px var(--color-text-title);
  }

  /* Footer */
  .footer {
    background-color: var(--color-text-title);
    color: white;
    padding: 70px 0 20px 0;
    border-top: 4px solid var(--color-orange);
  }

  .footer-content {
    display: grid;
    grid-template-columns: 1fr;
    gap: 40px;
    margin-bottom: 40px;
  }

  @media (min-width: 768px) {
    .footer-content {
      grid-template-columns: repeat(3, 1fr);
    }
  }

  .footer-brand {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  .footer-brand .logo-text {
    color: white;
  }

  .footer-motto {
    font-size: 0.95rem;
    color: #A0AEC0;
    margin-top: 15px;
  }

  .footer-links {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .footer-links h4,
  .footer-socials h4 {
    font-family: var(--font-header);
    font-size: 1.25rem;
    margin-bottom: 10px;
    color: var(--color-yellow);
  }

  .footer-links a {
    color: #D2D6DC;
    font-weight: 500;
  }

  .footer-links a:hover {
    color: var(--color-blue-light);
    transform: translateX(4px);
  }

  .footer-socials {
    display: flex;
    flex-direction: column;
    gap: 15px;
  }

  .social-icons {
    display: flex;
    gap: 15px;
  }

  .social-icon {
    width: 44px;
    height: 44px;
    background-color: rgba(255, 255, 255, 0.1);
    border: 2px solid white;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.5rem;
    transition: var(--transition-fast);
  }

  .social-icon:hover {
    background-color: var(--color-orange);
    transform: scale(1.1);
  }

  .footer-contact-text {
    font-size: 0.9rem;
    color: #A0AEC0;
  }

  .footer-bottom {
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    padding-top: 20px;
    text-align: center;
    font-size: 0.85rem;
    color: #A0AEC0;
  }
</style>