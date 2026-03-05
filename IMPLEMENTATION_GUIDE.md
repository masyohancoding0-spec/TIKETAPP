# 🎬 CinemaHub - Aplikasi Reservasi Tiket Bioskop Premium

## 📋 Ringkasan Implementasi

Anda kini memiliki **aplikasi reservasi tiket bioskop berbasis web** yang fully-featured dengan teknologi **React + Vite + Tailwind CSS**. Semua requirement telah diimplementasikan dengan fitur premium dan design yang mewah.

---

## 🏗️ Arsitektur Proyek

```
src/
├── AppNew.jsx                 # Main app component dengan semua fitur
├── context/
│   └── AppContext.jsx        # Global state management dengan Context API
├── components/
│   ├── MovieCard.jsx         # Kartu film dengan jadwal dan preview
│   ├── SeatSelection.jsx      # Interactive seat selection dengan 3D perspective
│   ├── SnackSelection.jsx     # Pemilihan snack & minuman add-ons
│   ├── PaymentGateway.jsx     # Payment processing (3 metode)
│   ├── TicketDisplay.jsx      # Tampilan tiket akhir dengan PDF download
│   └── AdminPanel.jsx         # CRUD management untuk film & jadwal
├── hooks/
│   └── useSeatManager.js      # Custom hook untuk seat locking & validation
├── utils/
│   ├── helpers.js            # Helper functions (validasi, payment sim, dll)
│   └── pdfGenerator.js        # PDF generation untuk tiket & nota
├── index.css                  # Premium Dark Cinema styling
└── main.jsx
```

---

## 🎯 Fitur Utama yang Diimplementasikan

### ✅ 1. SISI PENGGUNA (User Flow)

#### **Pilihan Jam & Film**
- Halaman beranda dengan grid film yang interaktif
- Search & filter film berdasarkan judul atau genre
- Preview setiap film dengan rating, durasi, dan sinopsis
- Tampilkan jadwal tayang tersedia untuk setiap film
- Tombol "Pesan Sekarang" untuk quick access

#### **Pilih Kursi (Seat Selection)**
- Layout grid kursi 5 baris × 8 kolom (A1-E8)
- **3D Perspective Screen** dengan efek glow cyan yang berpendar
- Status kursi dengan color coding:
  - 🟢 **Hijau**: Tersedia (available)
  - 🟡 **Kuning**: Dipilih pengguna (selected)
  - 🔴 **Merah**: Sudah terisi (booked)
  - 🟠 **Orange**: Terkunci saat transaksi (locked)
- **Validasi otomatis** untuk kursi yang sudah booked
- **Limit maksimal 5 kursi** per transaksi
- **Race condition prevention** dengan auto-lock mechanism (timeout 5 menit)
- Smooth animations saat pemilihan kursi

#### **Add-ons (Snack & Drink)**
- 6 opsi snack & minuman:
  - Popcorn (Regular/Large)
  - Soda (Regular/Large)
  - Nachos & Ice Cream
- Interface yang user-friendly dengan quantity controls
- Real-time price calculation

#### **Pembayaran (Checkout)**
- **3 metode pembayaran**:
  - ✅ **E-Wallet** (GCash, OVO, DANA, LinkAja)
  - ✅ **Bank Transfer** (BCA, Mandiri, BNI, CIMB)
  - ✅ **QRIS** (Universal QR Payment)
- Simulasi pembayaran yang realistis (95% success rate)
- Processing time yang natural (2-4 detik)
- Error handling yang proper

#### **Output & Cetak**
- ✅ **Tiket Digital** - Displayable dalam aplikasi
- ✅ **PDF Tiket** - Downloadable dengan styling premium
- ✅ **PDF Nota Pembayaran** - Receipt dengan breakdown harga
- QR-code simulation untuk validasi di bioskop

### ✅ 2. SISI ADMIN (Admin Panel & CRUD)

#### **Autentikasi Admin**
- Login modal dengan opsi "Admin" checkbox
- Akses admin panel terbatas hanya untuk pengguna dengan role admin
- Session management dengan localStorage

#### **Manajemen Film (CRUD)**
- ✅ **Tambah Film** - Input judul, durasi, deskripsi, rating
- ✅ **Upload Gambar** - Poster film dengan validasi:
  - Format: JPG/PNG
  - Ukuran max: 2MB
  - Preview sebelum upload
- ✅ **Edit Film** - Update semua data film
- ✅ **Hapus Film** - Dengan konfirmasi dan cascade delete jadwal

#### **Manajemen Jadwal**
- ✅ **Tambah Jadwal** - Set tanggal, jam tayang, harga tiket
- ✅ **Edit Jadwal** - Update informasi jadwal
- ✅ **Hapus Jadwal** - Dengan konfirmasi

---

## 🗄️ Database Schema (LocalStorage)

```javascript
// Movies
{
  id: number,
  title: string,
  genre: string,
  duration: number (menit),
  description: string,
  rating: number (0-5),
  posterUrl: string,
  releaseDate: string (ISO date)
}

// Schedules  
{
  id: number,
  movieId: number,
  date: string (YYYY-MM-DD),
  time: string (HH:MM),
  price: number (Rupiah)
}

// Seats (per schedule)
{
  [scheduleId]: {
    [seatCode]: {
      status: 'available' | 'booked' | 'locked',
      bookedAt: string (ISO datetime)
    }
  }
}

// Bookings
{
  id: string (BK-{timestamp}),
  userId: number,
  scheduleId: number,
  movieId: number,
  seats: string[],
  snacks: object[],
  totalAmount: number,
  status: 'pending' | 'confirmed',
  createdAt: string (ISO datetime)
}

// Transactions
{
  id: string (TRX-{timestamp}),
  bookingId: string,
  userId: number,
  amount: number,
  method: string,
  status: 'pending' | 'completed' | 'failed',
  createdAt: string (ISO datetime)
}
```

---

## 🛠️ Fitur Teknis Khusus

### ✅ Validasi Race Condition
- **Seat Locking Mechanism**: Kursi di-lock saat dipilih
- Auto-unlock setelah 5 menit jika transaksi tidak selesai
- Prevent double-booking dengan timestamp validation
- Lock tracking per schedule dengan unique lockId

### ✅ File Upload & Validation
- Validasi ukuran file (max 2MB)
- Validasi format (JPG/PNG only)
- Base64 storage di localStorage untuk persistence
- Preview image sebelum save

### ✅ PDF Export
- **jsPDF + html2canvas** untuk rendering PDF
- Styling premium dark Cinema di PDF
- Text-based PDF generation untuk nota pembayaran
- Download dengan naming convention (tiket-{bookingId}.pdf)

---

## 🎨 Design - Premium Dark Cinema Theme

### ✨ Efek Visual Mewah

#### **Glassmorphism**
```css
glassmorphism: {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.1);
}
```

#### **Golden Glow Accents**
- Warna aksen: `#FFD700` (bright gold) & `#D4AF37` (dark gold)
- Box-shadow dengan golden glow: `0 0 20px rgba(212, 175, 55, 0.5)`
- Active state dengan pulsing animation

#### **Deep Background Gradient**
```css
background: linear-gradient(135deg, #0f0f0f 0%, #232526 100%);
```

#### **3D Perspective Screen**
- Border-radius cinema: `50% / 30%`
- Gradient dari cyan: `rgba(34, 211, 238, 0.9)`
- Box-shadow yang berpendar
- Reflection effect dengan ::before pseudo-element

### 🎬 Animasi Dinamis (Micro-interactions)

#### **Smooth Seat Selection**
```css
@keyframes seatSelect {
  0% { transform: scale(1); }
  50% { transform: scale(1.15); }
  100% { transform: scale(1.1); }
}
```
Duration: 0.3s, dengan smooth color transition

#### **Hover Effects**
- Film cards: zoom pelan pada hover
- Buttons: scale dan glow shadow
- Input fields: ring dengan yellow accent pada focus

#### **Skeleton Loading Animation**
```css
@keyframes shimmer {
  0% { background-position: -1000px 0; }
  100% { background-position: 1000px 0; }
}
```

---

## 🚀 Cara Menggunakan

### **Untuk User Biasa:**

1. **Buka aplikasi** → Halaman beranda akan tampil dengan daftar film
2. **Cari film** → Gunakan search bar untuk filter
3. **Klik "Pesan Sekarang"** → Login jika belum (nama & email)
4. **Pilih Kursi** → Klik kursi untuk memilih (max 5)
5. **Tambah Snack** (optional) → Pilih snack yang diinginkan
6. **Pembayaran** → Pilih metode pembayaran
7. **Lihat Tiket** → Download PDF tiket & nota

### **Untuk Admin:**

1. **Login** → Centang "Login sebagai Admin"
2. **Klik Admin di header** → Masuk ke Admin Panel
3. **Tab Film**: Tambah/Edit/Hapus film dengan poster upload
4. **Tab Jadwal**: Manage jadwal tayang
5. Semua data otomatis disimpan ke localStorage

---

## 💾 Data Persistence

- Semua data tersimpan di **localStorage** secara otomatis
- Tidak perlu backend untuk demo
- Siap untuk integrasi dengan backend API

---

## 📦 Dependencies

```json
{
  "react": "^19.2.0",
  "react-dom": "^19.2.0",
  "lucide-react": "^0.575.0",
  "jspdf": "^4.2.0",
  "html2canvas": "^1.4.1",
  "tailwindcss": "^3.4.19",
  "vite": "^7.3.1"
}
```

---

## 🔧 Customization

### **Mengubah Warna Gold:**
Edit di `index.css`:
```css
--golden: #FFD700;  /* Change ke warna lain */
--golden-dark: #D4AF37;
```

### **Mengubah Jumlah Kursi:**
Edit di `AppContext.jsx` - `initialSchedules` dan seat layout

### **Mengubah Jumlah Max Seats:**
Edit di `useSeatManager.js`:
```javascript
const maxSeatsPerBooking = 5; // Change ke 10, 8, dll
```

### **Menambah Snack:**
Edit di `AppContext.jsx` - `snackOptions` array

---

## ⚠️ Catatan Penting

- Ini adalah **demonstrasi/prototype** dengan localStorage
- Untuk production, implementasikan backend dengan database (PostgreSQL/MongoDB)
- Session management perlu dikembangkan dengan proper authentication
- Payment gateway simulasi - gunakan provider real (Midtrans, Stripe, dll) untuk live

---

## 📝 File Structure Komplit

```
tiket-app/
├── src/
│   ├── AppNew.jsx
│   ├── App.old.jsx (backup)
│   ├── main.jsx
│   ├── index.css
│   ├── index.html
│   ├── context/
│   │   └── AppContext.jsx
│   ├── components/
│   │   ├── MovieCard.jsx
│   │   ├── SeatSelection.jsx
│   │   ├── SnackSelection.jsx
│   │   ├── PaymentGateway.jsx
│   │   ├── TicketDisplay.jsx
│   │   └── AdminPanel.jsx
│   ├── hooks/
│   │   └── useSeatManager.js
│   ├── utils/
│   │   ├── helpers.js
│   │   └── pdfGenerator.js
│   └── assets/
├── vite.config.js
├── tailwind.config.js
├── postcss.config.js
├── eslint.config.js
├── package.json
└── index.html
```

---

## 🎉 Fitur Bonus

- ✅ Dark mode default (Premium Dark Cinema)
- ✅ Responsive design (mobile, tablet, desktop)
- ✅ Search & filter functionality
- ✅ Real-time price calculation
- ✅ Booking history ("Tiket Saya")
- ✅ Smooth transitions & animations
- ✅ Error handling & validation
- ✅ User-friendly UI/UX

---

## 🎊 Kesimpulan

Aplikasi ini sudah **production-ready** dalam hal fitur dan design. Tinggal:
1. Integrasi dengan backend API
2. Setup real payment gateway
3. Add email notification
4. Deploy ke server

Selamat! 🎬🍿

