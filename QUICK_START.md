# 🚀 QUICK START GUIDE - CinemaHub

## 🎯 Mulai Menggunakan Aplikasi

### **Step 1: Eksplorasi Halaman Beranda**
- Aplikasi akan terbuka dengan default state
- Anda akan melihat halaman "Selamat Datang" dengan grid film
- Gunakan search bar untuk mencari film

### **Step 2: Login (Required untuk menonton)**
- Klik tombol LOGIN atau salah satu film
- Masukkan **Nama** dan **Email**
- Checkbox "Login sebagai Admin" hanya jika ingin akses admin panel
- Klik tombol **MASUK**

### **Step 3: Pilih Film & Jadwal**
- Klik tombol **"Pesan Sekarang"** pada film yang diinginkan
- Atau klik langsung jam pada preview film
- Anda akan diarahkan ke step pemilihan kursi

### **Step 4: Pilih Kursi (Interactive Seat Map)**
- Lihat layout kursi dengan **3D perspective cinema screen** di atas
- **Warna Kursi:**
  - 🟢 Hijau = Tersedia → Klik untuk pilih
  - 🟡 Kuning = Dipilih Anda (dengan glow effect)
  - 🔴 Merah = Sudah terisi (disabled)
  - 🟠 Orange = Terkunci saat org lain booking

- **Pilih maksimal 5 kursi**
- Kursi akan di-lock otomatis saat dipilih (5 menit timeout)
- Klik **LANJUTKAN** setelah selesai

### **Step 5: Tambah Snack (Optional)**
- Lihat 6 pilihan snack & minuman dengan harga
- Tekan **+** untuk menambah, **-** untuk kurangi
- Harga dihitung real-time
- Edit nanti di step payment juga bisa
- Klik **LANJUT KE PEMBAYARAN**

### **Step 6: Pilih Metode Pembayaran**
- **3 Pilihan:**
  1. 💳 **E-Wallet** → Pilih provider (GCash, OVO, DANA, LinkAja)
  2. 🏦 **Bank Transfer** → Pilih bank
  3. 📱 **QRIS** → QR Universal Payment

- Klik method yang ingin digunakan
- Provider akan muncul otomatis
- Klik **LANJUTKAN PEMBAYARAN**
- Tunggu proses simulasi (2-4 detik)

### **Step 7: Lihat & Download Tiket**
- ✅ Pembayaran berhasil!
- Lihat **TIKET DIGITAL** di layar dengan detail:
  - Film name & genre
  - Jadwal tayang
  - Nomor kursi (dengan styling golden)
  - Snack yang dibeli
  - Total harga bayar

- **Download PDF:**
  - 📄 **Tiket PDF** - Untuk ditunjukkan saat entry bioskop
  - 📋 **Nota PDF** - Bukti pembayaran dengan breakdown

- Klik **SELESAI** untuk kembali ke halaman Tiket Saya

### **Step 8: Cek Tiket Saya**
- Di menu header, klik **"Tiket Saya"**
- Lihat semua booking Anda dengan:
  - Nama film
  - Tanggal & jam tayang
  - Nomor kursi
  - Harga pembayaran

---

## 👨‍💼 ADMIN FEATURES

### **Login sebagai Admin:**
1. Di login modal, **centang kotak "Login sebagai Admin"**
2. Masukkan nama & email
3. Klik MASUK

### **Akses Admin Panel:**
1. Di header, akan muncul menu **"👨‍💼 Admin Panel"**
2. Klik untuk masuk ke admin area

### **Manajemen Film (Tab 🎬 Film):**

**Tambah Film Baru:**
- Klik **"+ Tambah Film Baru"**
- Fill form:
  - Judul Film *
  - Genre *
  - Durasi (menit) *
  - Rating (0-5)
  - Deskripsi
- Upload Poster:
  - Click "Klik untuk upload"
  - Pilih file JPG/PNG (max 2MB)
  - Preview gambar akan tampil
- Klik **SIMPAN**

**Edit Film:**
- Klik tombol **EDIT** pada film card
- Form akan terisi dengan data lama
- Edit sesuai kebutuhan
- Ubah/update poster jika perlu
- Klik **PERBARUI**

**Hapus Film:**
- Klik tombol **HAPUS** pada film card
- Konfirmasi akan diminta
- Film dan semua jadwalnya akan dihapus

### **Manajemen Jadwal (Tab ⏰ Jadwal):**

**Tambah Jadwal:**
- Klik **"+ Tambah Jadwal"**
- Film * → Pilih dari dropdown
- Tanggal * → Pilih tanggal
- Jam Tayang * → Set jam (HH:MM format)
- Harga Tiket * → Harga dalam Rupiah
- Klik **SIMPAN**

**Edit Jadwal:**
- Klik tombol **EDIT** (icon pensil)
- Edit data yang ingin diubah
- Klik **PERBARUI**

**Hapus Jadwal:**
- Klik tombol **HAPUS** (icon trash)
- Konfirmasi diminta
- Jadwal akan dihapus

---

## 📱 INTERFACE TIPS

### **Desktop (Recommended):**
- Lihat semua navigasi di header
- Layout optimal untuk seat selection
- PDF download lebih smooth

### **Mobile:**
- Hamburger menu (≡) untuk navigasi
- Responsive layout otomatis adjust
- Touch-friendly buttons
- Scroll untuk lihat semua konten

### **Dark Mode:**
- Default dark theme (Premium Dark Cinema)
- Golden accents & glow effects
- Optimized untuk mata di ruang gelap

---

## 🎨 STYLING HIGHLIGHTS

### **Color Scheme:**
- 🟢 **Emerald**: Available seats (#10b981)
- 🟡 **Gold/Yellow**: Selected items (#fbbf24, #FFD700)
- 🔴 **Red**: Booked/unavailable (#dc2626)
- 🔵 **Cyan/Blue**: Screen & primary actions
- ⚫ **Dark Slate**: Background & cards
- ⚪ **White/Gray**: Text & borders

### **Animations:**
- Smooth transitions (0.2s-0.3s)
- Hover effects dengan scale
- Glow box-shadow pada golden elements
- Shimmer loading placeholder
- Fade-in saat navigate

### **Premium Effects:**
- 🎆 **Glassmorphism**: Blur backdrop pada modals & cards
- ✨ **Golden Glow**: Accent dengan shadow glow
- 🎬 **Cinema Screen**: 3D perspective curve di bagian atas seat map
- 📦 **Smooth Cards**: Gradient backgrounds dengan subtle borders

---

## ⚙️ SETTINGS & PREFERENCES

### **Local Storage:**
- Data otomatis tersimpan di browser
- Movies, schedules, bookings, transactions
- Tidak perlu server untuk demo
- Clear cache/cookies akan reset semua data

### **Session Management:**
- Logout dengan klik icon LogOut (⏐⏐) di header
- Auto-logout tidak ada (stay logged in)
- Multiple login sessions bisa (tapi hanya 1 user aktif)

---

## 🐛 TROUBLESHOOTING

### **Q: Kursi tidak bisa dipilih?**
- **A**: Kursi sudah terisi atau terkunci. Tunggu 5 menit atau pilih kursi lain.

### **Q: Pembayaran gagal?**
- **A**: Ini simulasi dengan 95% success rate. Coba ulang atau ganti metode pembayaran.

### **Q: File upload poster tidak berhasil?**
- **A**: Pastikan format JPG/PNG dan ukuran ≤ 2MB. Check console untuk error detail.

### **Q: Tiket tidak bisa didownload?**
- **A**: Gunakan browser Chrome/Firefox terbaru. Pop-up blocker mungkin block PDF.

### **Q: Data hilang setelah refresh?**
- **A**: Data disimpan di localStorage. Jika cache dihapus, data akan hilang. Backup dengan export PDF.

---

## 📊 DATA SAMPLE

Aplikasi sudah tersedia dengan data sample:

**3 Film Ready:**
- Gundala: Putra Petir (Action)
- Mencuri Raden Saleh (Heist)
- Pengabdi Setan 2 (Horror)

**6 Jadwal Tersedia**
- Berbagai jam tayang untuk setiap film
- Harga berkisar 40-55K per tiket

**6 Snack Options**
- Popcorn, Soda, Nachos, Ice Cream
- Price range 10K-35K

---

## 🎊 ENJOY!

Anda siap menggunakan **CinemaHub** - Aplikasi Reservasi Tiket Bioskop Premium!

Untuk lebih detail, baca **IMPLEMENTATION_GUIDE.md**

Pertanyaan? Cek console browser (F12) untuk error messages.

**Happy Booking!** 🎬🍿✨
