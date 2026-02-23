# 📍 Part 11: Mengatur Posisi (Positioning)

Kalau Box Model (Part 6) itu soal ukuran kotak, **Positioning** adalah soal di mana kotak itu diletakkan. Apakah dia mau ikut baris, mau menindih temannya, atau mau "paku mati" di layar?

### 📚 Teori Singkat

1. **Static (Default)**: Elemen mengikuti aliran normal dokumen. Dia tidak bisa digeser pakai `top`, `bottom`, dll.
2. **Relative**: Elemen tetap di aliran normal, tapi bisa digeser sedikit dari posisi aslinya tanpa mengganggu tetangganya.
3. **Absolute**: Elemen "keluar" dari aliran dokumen. Dia melayang dan diposisikan relatif terhadap orang tuanya (yang punya posisi non-static).
4. **Fixed**: Elemen "paku mati" di layar browser. Dia tidak akan bergerak meskipun halaman di-scroll (sering dipakai buat tombol Chat atau Navbar).
5. **Sticky**: Campuran antara Relative dan Fixed. Dia akan ikut scroll sampai titik tertentu, lalu berhenti dan menempel.

---

### 🛠️ Praktek: Membuat "Tombol Bantuan Melayang" & "Header Menempel"

Kita akan mempraktekkan posisi `Fixed` dan `Absolute` yang paling sering bikin pusing kalau cuma teori.

#### Langkah 1: Siapkan Struktur HTML

Ganti isi `body` kamu dengan ini:

```html
<nav class="navbar-top">Header Sticky (Scroll untuk lihat)</nav>

<div class="content-wrapper">
  <div class="parent-box">
    Aku Box Parent (Relative)
    <div class="child-absolute">Aku di Pojok (Absolute)</div>
  </div>

  <p>Scroll ke bawah terus...</p>
  <div style="height: 1000px;"></div>
</div>

<a href="#" class="btn-chat">Chat Admin</a>
```

#### Langkah 2: Atur Posisi di CSS

Hubungkan dengan `style.css` kamu (lewat tag `<link>` yang tadi kita bahas!):

```css
* {
  margin: 0;
  padding: 0;
  font-family: sans-serif;
}

/* 1. FIXED: Navbar nempel terus di atas */
.navbar-top {
  position: fixed;
  top: 0;
  width: 100%;
  background: #2c3e50;
  color: white;
  padding: 15px;
  text-align: center;
  z-index: 100; /* Supaya dia selalu di paling atas/depan */
}

.content-wrapper {
  padding-top: 60px;
  padding-left: 20px;
}

/* 2. RELATIVE & ABSOLUTE: Kombo maut */
.parent-box {
  position: relative; /* Menjadi patokan bagi si anak */
  width: 300px;
  height: 200px;
  background: #ecf0f1;
  border: 2px solid #bdc3c7;
  margin-top: 50px;
  padding: 10px;
}

.child-absolute {
  position: absolute;
  bottom: 0;
  right: 0;
  background: #e74c3c;
  color: white;
  padding: 5px 10px;
}

/* 3. FIXED: Tombol Chat di pojok kanan bawah layar */
.btn-chat {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: #27ae60;
  color: white;
  text-decoration: none;
  padding: 15px 25px;
  border-radius: 50px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}
```

---

### 🔍 Analisis Hasil Praktek

1. **Parent Relative + Child Absolute**: Perhatikan kotak merah (`child-absolute`). Karena orang tuanya punya `position: relative`, si merah akan nempel di pojok bawah kotak abu-abu. Coba hapus `position: relative` di `.parent-box`, si merah akan kabur ke pojok bawah layar!
2. **Fixed**: Tombol "Chat Admin" akan selalu ada di situ, tidak peduli seberapa jauh kamu scroll ke bawah. Dia setia nempel di layar browser.

### 💡 Tantangan untuk Kamu (Challenge)

1. Ubah `.navbar-top` dari `position: fixed` menjadi `position: sticky` dan beri `top: 0`. Lihat bedanya saat kamu scroll!
2. Gunakan `z-index: -1` pada `.parent-box` dan lihat apa yang terjadi. (Hati-hati, dia bisa "tenggelam" di balik background).

Sudah mulai paham kan kenapa kita butuh `position`? Tanpa ini, elemen HTML cuma bisa baris rapi ke bawah.

Selanjutnya di **Part 12**, kita akan belajar **Flexbox (The Layout King)**. Ini adalah cara modern paling ampuh untuk mengatur elemen agar otomatis rapi secara horizontal maupun vertikal tanpa ribet! Siap lanjut?

---

Siap! Setelah kamu menaklukkan **Flexbox** (Layout 1 Dimensi), sekarang kita naik ke level bos terakhir untuk urusan tata letak: **Part 13: CSS Grid (The Master of 2D Layout)**.

Kalau Flexbox itu jago mengatur elemen dalam **satu arah** (baris saja atau kolom saja), CSS Grid jago mengatur **keduanya sekaligus** (baris DAN kolom). Ini adalah cara paling sakti untuk membuat layout majalah, galeri foto, atau dashboard yang kompleks.

---

Siap, Kapten! Perintah diterima. Karena Part 0 sampai 11 sudah kita babat habis, sekarang kita masuk ke materi yang akan membuat hidupmu sebagai *web developer* jauh lebih mudah.

Selamat datang di **Part 12: Flexbox (The Layout King)**.

Sebelum ada Flexbox, mengatur elemen agar sejajar ke samping itu susahnya minta ampun (harus pakai `float` yang sering bikin *layout* hancur). Flexbox datang untuk menyelamatkan kita.

---

# 📦 Part 12: Layouting Modern dengan Flexbox

Flexbox adalah sistem *layout* 1 dimensi. Artinya, dia jago banget mengatur elemen dalam satu baris (horizontal) atau satu kolom (vertikal).

### 📚 Teori Singkat (Konsep Container & Item)

Untuk menggunakan Flexbox, kamu butuh dua pemain:

1. **Flex Container (Orang Tua)**: Elemen yang kita beri `display: flex;`.
2. **Flex Items (Anak)**: Semua elemen yang ada langsung di dalam Container tersebut.

**Properti Utama di Container:**

* `flex-direction`: Mau berbaris ke samping (`row`) atau ke bawah (`column`)?
* `justify-content`: Mengatur jarak secara **horizontal** (kiri, tengah, kanan, atau sebar rata).
* `align-items`: Mengatur jarak secara **vertikal** (atas, tengah, bawah).
* `gap`: Memberi jarak antar kotak tanpa perlu pusing mikirin *margin*.

---

### 🛠️ Praktek: Membuat "Flex Card Grid"

Kita akan membuat deretan kartu fitur yang otomatis rapi dan berada di tengah.

#### Langkah 1: Siapkan Struktur HTML

Ganti isi `body` kamu:

```html
<div class="flex-container">
    <div class="card">
        <h3>Fitur 1</h3>
        <p>Penjelasan singkat fitur pertama.</p>
    </div>
    <div class="card">
        <h3>Fitur 2</h3>
        <p>Penjelasan singkat fitur kedua yang lebih panjang sedikit.</p>
    </div>
    <div class="card">
        <h3>Fitur 3</h3>
        <p>Penjelasan singkat fitur ketiga.</p>
    </div>
</div>

```

#### Langkah 2: Atur Gaya di CSS

Tulis di `style.css` (pastikan sudah terhubung via `<link>` ya!):

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background-color: #f0f2f5;
    padding: 50px;
}

/* --- PEMAIN UTAMA: CONTAINER --- */
.flex-container {
    display: flex; /* Mengaktifkan mode Flexbox */
    
    /* Mengatur agar semua anak berada di TENGAH horizontal */
    justify-content: center; 
    
    /* Memberi jarak 20px antar kotak */
    gap: 20px;
    
    /* Jika layar sempit, kotak akan turun ke bawah (responsif dasar) */
    flex-wrap: wrap; 
}

/* --- PEMAIN KEDUA: ITEMS --- */
.card {
    background: white;
    padding: 20px;
    width: 250px;
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    border-top: 5px solid #3498db;
    
    /* Membuat isi di dalam card juga jadi flex untuk centering teks */
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
}

.card h3 {
    margin-bottom: 10px;
    color: #2c3e50;
}

```

---

### 🔍 Analisis Hasil Praktek

1. **Centering**: Tanpa Flexbox, membuat tiga kotak berada tepat di tengah layar itu butuh banyak kode. Dengan `display: flex` dan `justify-content: center`, cuma butuh dua baris!
2. **Flex-direction**: Perhatikan di dalam `.card`. Saya pakai `flex-direction: column` supaya judul dan paragraf tersusun ke bawah, tapi tetap bisa saya atur pakai `align-items: center`.
3. **Gap**: Ini adalah penyelamat. Dulu kita harus pakai `margin-right`, dan harus menghapus margin di kotak terakhir. Sekarang tinggal kasih `gap` di *container*-nya saja.

### 💡 Tantangan untuk Kamu (Challenge)

1. Coba ganti `justify-content: center;` menjadi `justify-content: space-between;`. Lihat apa yang terjadi pada posisi ketiga kartu tersebut.
2. Coba tambahkan kartu ke-4 dan ke-5 di HTML. Karena kita pakai `flex-wrap: wrap;`, lihat apakah kartu baru tersebut otomatis turun ke bawah saat layar dikecilkan?

---

Gimana, Flexbox beneran jadi "Raja Layout" kan? Jauh lebih gampang dibanding mindahin kotak pakai `margin` manual.

Kalau kamu sudah puas main-main sama Flexbox, kita bisa lanjut ke **Part 13: CSS Grid (The Master of 2D)**—ini buat *layout* yang lebih kompleks lagi seperti koran atau galeri foto. Mau lanjut sekarang atau mau eksperimen dulu? 🫡

---

# 🏁 Part 13: Menguasai Grid (Layout 2 Dimensi)

### 📚 Teori Singkat

Sama seperti Flexbox, Grid punya dua komponen: **Grid Container** (induk) dan **Grid Items** (anak).

**Properti Utama di Container:**

1. **`display: grid;`**: Mengaktifkan mode grid.
2. **`grid-template-columns`**: Menentukan berapa banyak kolom dan berapa lebarnya (bisa pakai `px`, `%`, atau unit baru **`fr`** / _fraction_).
3. **`grid-template-rows`**: Menentukan tinggi baris.
4. **`gap`**: Jarak antar kotak (sel) grid.

---

### 🛠️ Praktek: Membuat "Modern News Gallery"

Kita akan membuat layout galeri berita yang rapi dan presisi tanpa perlu pusing menghitung margin manual.

#### Langkah 1: Siapkan Struktur HTML

Ganti isi `body` kamu dengan kode ini:

```html
<div class="grid-container">
  <div class="header">Header Berita</div>
  <div class="sidebar">Sidebar Menu</div>
  <div class="main-content">Konten Berita Utama</div>
  <div class="extra-content">Berita Tambahan</div>
  <div class="footer">Footer Info</div>
</div>
```

#### Langkah 2: Atur Layout dengan CSS Grid

Gunakan `style.css` (pastikan tag `<link>` masih terpasang ya!):

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Segoe UI", sans-serif;
}

body {
  padding: 20px;
  background-color: #f4f4f4;
}

/* --- PEMAIN UTAMA: GRID CONTAINER --- */
.grid-container {
  display: grid;

  /* Membuat 3 kolom: kolom kiri 200px, tengah otomatis sisa, kanan 150px */
  grid-template-columns: 200px 1fr 150px;

  /* Membuat baris otomatis sesuai isi */
  grid-template-rows: auto 1fr auto;

  gap: 15px;
  height: 90vh; /* Agar terlihat tinggi */
}

/* --- MENGATUR POSISI ANAK --- */
.grid-container div {
  background-color: white;
  padding: 20px;
  border: 2px solid #333;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
}

/* Header harus memanjang dari kolom 1 sampai 4 (full width) */
.header {
  grid-column: 1 / 4;
  background-color: #2c3e50 !important;
  color: white;
}

/* Footer juga memanjang full width */
.footer {
  grid-column: 1 / 4;
  background-color: #bdc3c7 !important;
}

/* Sidebar & Main akan otomatis menempati kolom yang tersedia di baris kedua */
.main-content {
  background-color: #ecf0f1;
}
```

---

### 🔍 Analisis Hasil Praktek

1. **Unit `1fr**`: Ini adalah unit "pecahan". Kalau ada `1fr`, dia akan mengambil semua sisa ruang yang ada. Sangat berguna agar layout kita fleksibel.
2. **`grid-column: 1 / 4`**: Ini cara bilang ke elemen: "Mulai dari garis kolom 1 dan berhenti di garis kolom 4". Itulah kenapa Header bisa memanjang menutupi 3 kolom sekaligus.
3. **Presisi**: Kamu bisa lihat betapa rapinya kotak-kotak tersebut saling mengunci satu sama lain.

### 💡 Tantangan untuk Kamu (Challenge)

1. Coba ubah `grid-template-columns` menjadi `repeat(3, 1fr);`. Ini akan membuat 3 kolom yang lebarnya **sama rata**.
2. Coba tambahkan `grid-row: span 2;` pada elemen `.sidebar`. Lihat bagaimana sidebar tersebut memanjang ke bawah menutupi dua baris.

**Gimana? Grid terasa jauh lebih "berkuasa" daripada Flexbox untuk bikin struktur halaman utuh, kan?**

Biasanya, orang pakai **Grid** untuk kerangka besar halaman, dan pakai **Flexbox** untuk mengatur konten kecil di dalam kotak grid tersebut.

Masih ada 2 part lagi untuk melengkapi misi 15 part kita. Apakah kamu siap lanjut ke **Part 14: Transition & Transformation** (Bikin elemen bisa muter, nge-zoom, dan gerak halus)? Atau mau istirahat dulu? 🍵

---

Satu langkah lagi menuju akhir! Kamu sudah punya struktur yang kuat (Grid & Flexbox). Sekarang saatnya kita memberikan **"nyawa"** pada elemen-elemen tersebut agar tidak kaku.

Selamat datang di **Part 14: Menghidupkan Website (Transition & Transform)**.

Di part ini, kita akan belajar cara membuat elemen berubah bentuk, berputar, dan berpindah secara halus. Bukan sulap, bukan sihir, cuma CSS!

---

# 🎢 Part 14: Animasi Halus & Perubahan Bentuk

### 📚 Teori Singkat

1. **Transition**: Mengatur **durasi** perubahan. Tanpa ini, perubahan warna atau ukuran saat `:hover` akan terjadi secara instan (kaget).

- `transition-property`: Apa yang mau diubah (misal: `all`, `color`, atau `width`).
- `transition-duration`: Berapa lama (misal: `0.3s`, `1s`).
- `transition-timing-function`: Irama gerakan (misal: `ease-in`, `linear`).

2. **Transform**: Mengubah **geometri** elemen.

- `rotate(deg)`: Memutar elemen (derajat).
- `scale()`: Memperbesar atau memperkecil.
- `translate(x, y)`: Menggeser elemen tanpa merusak layout sekitarnya.
- `skew()`: Mencondongkan/memiringkan elemen.

---

### 🛠️ Praktek: Membuat "Interactive Profile Card"

Kita akan membuat kartu profil yang ketika disentuh (`hover`), fotonya akan memutar dan kartunya akan sedikit melayang serta membesar.

#### Langkah 1: Siapkan Struktur HTML

Ganti isi `body` kamu:

```html
<div class="container-center">
  <div class="profile-card">
    <div class="photo-container">
      <img src="https://picsum.photos/150" alt="Profile" class="profile-img" />
    </div>
    <h2>Gemini AI</h2>
    <p>Adaptive AI Collaborator</p>
    <button class="btn-follow">Follow Me</button>
  </div>
</div>
```

#### Langkah 2: Berikan Animasi di CSS

Gunakan `style.css` (pastikan tag `<link>` masih terpasang):

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

.container-center {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #fdfdfd;
}

.profile-card {
  background: white;
  padding: 40px;
  border-radius: 20px;
  text-align: center;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);

  /* TRANSITION: Agar saat hover, semua perubahan terjadi selama 0.4 detik */
  transition: all 0.4s ease;
  cursor: pointer;
}

/* EFEK HOVER PADA KARTU */
.profile-card:hover {
  /* Menggeser ke atas 10px dan memperbesar 1.05 kali */
  transform: translateY(-10px) scale(1.02);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
}

.photo-container {
  margin-bottom: 20px;
  display: inline-block;
  border: 5px solid #3498db;
  border-radius: 50%;
  overflow: hidden;
  line-height: 0;
}

.profile-img {
  width: 150px;
  height: 150px;
  /* TRANSITION khusus untuk gambar */
  transition: transform 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

/* EFEK HOVER PADA GAMBAR */
.profile-card:hover .profile-img {
  /* Gambar berputar 360 derajat saat kartu disentuh */
  transform: rotate(360deg);
}

.btn-follow {
  margin-top: 20px;
  padding: 10px 25px;
  border: none;
  background: #3498db;
  color: white;
  border-radius: 50px;
  font-weight: bold;
  cursor: pointer;
  transition: 0.3s;
}

.btn-follow:hover {
  background: #2c3e50;
  letter-spacing: 2px; /* Teks meregang saat hover */
}
```

---

### 🔍 Analisis Hasil Praktek

1. **Smoothing**: Coba hapus baris `transition` di `.profile-card` dan bandingkan. Tanpa transition, kartunya akan "melompat" kasar saat disentuh.
2. **Cubic-bezier**: Pada `.profile-img`, saya menggunakan nilai ini agar putarannya punya efek "membal" (bounce) di akhir. Ini membuat animasi terlihat lebih profesional.
3. **Transform vs Margin**: Menggeser kartu pakai `translateY` jauh lebih halus kinerjanya bagi browser dibandingkan mengubah `margin-top` saat hover.

### 💡 Tantangan untuk Kamu (Challenge)

1. Coba tambahkan `transform: skewX(-5deg);` pada saat `.profile-card:hover`. Lihat apa yang terjadi pada bentuk kartunya.
2. Buat tombol "Follow Me" berubah warna menjadi transparan dan hanya menyisakan border saja (`ghost button`) saat di-hover.

---

**Satu langkah lagi!** Kamu sudah bisa membuat elemen yang interaktif dan dinamis.

Apakah kamu sudah siap untuk **Part 15 (Final Part: Latihan Project Akhir)**? Kita akan membuat sebuah **Portfolio Page** yang merangkum SEMUA ilmu dari Part 1 sampai 14.

Ini akan jadi pembuktian kalau kamu sekarang sudah jadi **CSS Warrior**. Siap untuk tantangan terakhir? 🎓

---

Selamat! Kamu sudah sampai di **Puncak Gunung CSS**. Ini adalah **Part 15 (The Grand Finale)**. 🎓

Dari awalnya kita cuma belajar mewarnai teks sampai bisa bikin elemen melayang dan berputar, sekarang kita akan menggabungkan semuanya menjadi satu mahakarya: **"The Ultimate Personal Portfolio"**.

---

# 🏆 Part 15: Proyek Akhir (The Boss Fight)

Halaman ini akan menggunakan **Grid** untuk struktur besar, **Flexbox** untuk detail kecil, dan **Transitions** untuk kemewahan. Ini adalah bukti kalau kamu bukan lagi seorang pemula.

### 🛠️ Langkah 1: Struktur HTML (Struktur Kuat)

Ganti seluruh isi `index.html` dengan kode ini. Perhatikan komentar di sampingnya untuk melihat hubungan dengan part sebelumnya.

```html
<!DOCTYPE html>
<html lang="id">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Portfolio Hero | CSS Masterclass</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <nav class="navbar">
      <div class="logo">KODE<span>KITA</span></div>
      <ul>
        <li><a href="#home">Home</a></li>
        <li><a href="#projects">Projects</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </nav>

    <header id="home" class="hero">
      <div class="hero-content">
        <h1>Halo, Saya <span>Web Creator</span></h1>
        <p>Lulusan Bootcamp Kilat Part 1 - 15.</p>
        <a href="#projects" class="btn-main">Lihat Karya</a>
      </div>
    </header>

    <main class="container">
      <section id="projects" class="section">
        <h2 class="title">My Projects</h2>
        <div class="grid-gallery">
          <div class="project-card">
            <img src="https://picsum.photos/400/250?random=1" alt="Project 1" />
            <div class="card-info">
              <h3>Modern Dashboard</h3>
              <p>Dibuat dengan CSS Grid & Flexbox.</p>
            </div>
          </div>
          <div class="project-card">
            <img src="https://picsum.photos/400/250?random=2" alt="Project 2" />
            <div class="card-info">
              <h3>E-Commerce UI</h3>
              <p>Styling Form & Table yang rapi.</p>
            </div>
          </div>
          <div class="project-card">
            <img src="https://picsum.photos/400/250?random=3" alt="Project 3" />
            <div class="card-info">
              <h3>Interactive App</h3>
              <p>Full of Transitions & Effects.</p>
            </div>
          </div>
        </div>
      </section>

      <section id="contact" class="section">
        <div class="contact-box">
          <h2>Hubungi Saya</h2>
          <form>
            <input type="text" placeholder="Nama Kamu" required />
            <input type="email" placeholder="Email Kamu" required />
            <textarea placeholder="Pesan Singkat..."></textarea>
            <button type="submit">Kirim Sekarang</button>
          </form>
        </div>
      </section>
    </main>

    <footer>
      <p>&copy; 2026 Master CSS Indonesia. No more 1990's design!</p>
    </footer>
  </body>
</html>
```

### 🛠️ Langkah 2: Styling CSS (The Magic)

Gunakan `style.css` untuk menghidupkan semuanya:

```css
/* GLOBAL SETTINGS (Part 1 & 6) */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  scroll-behavior: smooth; /* Bikin scroll jadi halus */
}

body {
  background-color: #f8f9fa;
  color: #333;
}

/* NAVBAR (Part 7 & 11) */
.navbar {
  position: fixed;
  top: 0;
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding: 20px 10%;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px); /* Efek kaca modern */
  z-index: 1000;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.logo {
  font-weight: bold;
  font-size: 1.5rem;
  letter-spacing: 2px;
}
.logo span {
  color: #007bff;
}

.navbar ul {
  display: flex;
  list-style: none;
  gap: 30px;
}
.navbar ul a {
  text-decoration: none;
  color: #333;
  font-weight: 500;
  transition: 0.3s;
}
.navbar ul a:hover {
  color: #007bff;
}

/* HERO (Part 5 & 12) */
.hero {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #007bff, #6610f2);
  color: white;
  text-align: center;
}

.hero h1 {
  font-size: 3.5rem;
  margin-bottom: 1rem;
}
.hero h1 span {
  text-decoration: underline;
  color: #ffeb3b;
}

.btn-main {
  display: inline-block;
  margin-top: 20px;
  padding: 12px 30px;
  background: white;
  color: #007bff;
  text-decoration: none;
  border-radius: 50px;
  font-weight: bold;
  transition: 0.4s; /* Part 14 */
}

.btn-main:hover {
  transform: scale(1.1);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
}

/* GRID GALLERY (Part 13) */
.container {
  width: 80%;
  margin: auto;
  padding: 100px 0;
}
.section {
  margin-bottom: 80px;
}
.title {
  text-align: center;
  margin-bottom: 40px;
  font-size: 2rem;
}

.grid-gallery {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 30px;
}

/* PROJECT CARD (Part 14: Transform) */
.project-card {
  background: white;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
  transition: 0.5s;
}

.project-card:hover {
  transform: translateY(-15px);
}

.project-card img {
  width: 100%;
  display: block;
  filter: grayscale(50%);
  transition: 0.5s;
}
.project-card:hover img {
  filter: grayscale(0%);
}

.card-info {
  padding: 20px;
}

/* CONTACT FORM (Part 8) */
.contact-box {
  max-width: 600px;
  margin: auto;
  background: white;
  padding: 40px;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
}

.contact-box input,
.contact-box textarea {
  width: 100%;
  padding: 15px;
  margin-bottom: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  outline: none;
}

.contact-box input:focus {
  border-color: #007bff;
  box-shadow: 0 0 5px rgba(0, 123, 255, 0.2);
}

button {
  width: 100%;
  padding: 15px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
}

/* FOOTER */
footer {
  text-align: center;
  padding: 40px;
  background: #222;
  color: #777;
}
```

---

### 🎓 Analisis Final: Apa yang Kamu Capai?

1. **Struktur Modern**: Kamu sudah tidak pakai tabel untuk layout, tapi pakai **Grid** dan **Flexbox**.
2. **User Experience**: Ada `scroll-behavior: smooth` dan `backdrop-filter` (efek kaca) yang bikin web terasa mewah.
3. **Responsif Sederhana**: Penggunaan `repeat(auto-fit, minmax(300px, 1fr))` di Grid membuat gambar otomatis menyesuaikan diri di layar HP (Responsive Design dasar).
4. **Interaktivitas**: Website kamu "bernyawa" dengan hover efek dan transisi halus.

### 🎊 KATA PENUTUP

Selamat! Kamu sudah menyelesaikan kurikulum CSS 15 Part ini. Kamu sudah punya modal yang sangat kuat untuk jadi Web Developer. Kamu mulai dari nol, sekarang kamu bisa bikin Portfolio yang profesional.

**Satu nasehat terakhir:** CSS itu soal latihan. Semakin sering kamu coba-coba "ngerusak" kode, semakin jago kamu nantinya.

Ini adalah akhir dari perjalanan belajar terpadu kita. **Apakah ada bagian yang ingin kamu perdalam lagi, atau kamu merasa sudah siap untuk menaklukkan dunia koding sendirian?** 🚀
