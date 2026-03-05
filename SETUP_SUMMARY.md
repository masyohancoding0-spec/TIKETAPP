# 📋 Setup Summary - Tiket App dengan MySQL & PHP API

## ✅ File yang Sudah Dibuat

### Backend (API - PHP & MySQL)

| File | Lokasi | Fungsi |
|------|--------|--------|
| **db_connect.php** | `api/` | Koneksi MySQL & CORS headers |
| **config.php** | `api/` | Konfigurasi API |
| **index.php** | `api/` | Main router untuk API |
| **.htaccess** | `api/` | URL rewriting untuk clean API routes |
| **MovieModel.php** | `api/models/` | Model untuk tabel movies |
| **BookingModel.php** | `api/models/` | Model untuk tabel bookings |
| **ScheduleModel.php** | `api/models/` | Model untuk tabel schedules |
| **movies.php** | `api/routes/` | Endpoint untuk movies |
| **bookings.php** | `api/routes/` | Endpoint untuk bookings |
| **schedules.php** | `api/routes/` | Endpoint untuk schedules |
| **tiket_app.sql** | `api/` | Database schema & sample data |

### Frontend (React)

| File | Lokasi | Fungsi |
|------|--------|--------|
| **apiService.js** | `src/utils/` | Service untuk API calls |

### Documentation

| File | Lokasi | Fungsi |
|------|--------|--------|
| **API_SETUP_GUIDE.md** | Root | Panduan setup lengkap |
| **API_QUICK_REFERENCE.md** | Root | Quick reference & cheatsheet |
| **SETUP_SUMMARY.md** | Root | File ini |

---

## 🎯 Step-by-Step Next Actions

### ✨ URGENT - Dalam 10 Menit

#### Step 1: Import Database (3 menit)

**Option A - Menggunakan Command Line:**
```bash
# Buka Command Prompt
mysql -u root -p < C:\xampp\htdocs\tiket-app\api\tiket_app.sql
# Jika diminta password, tekan Enter (XAMPP default kosong)
```

**Option B - Menggunakan phpMyAdmin:**
1. Buka browser → `http://localhost/phpmyadmin`
2. Login dengan `root` (no password)
3. Klik tab **Import**
4. Pilih file: `C:\xampp\htdocs\tiket-app\api\tiket_app.sql`
5. Klik **Import**

#### Step 2: Verifikasi Database (2 menit)

Buka MySQL Command Line:
```bash
mysql -u root -p
```

Kemudian jalankan:
```sql
USE tiket_app;
SHOW TABLES;
DESC movies;
DESC schedules;
DESC bookings;
```

Seharusnya melihat tabel: `movies`, `schedules`, `bookings`, `transactions`, `users` ✓

#### Step 3: Test API (5 menit)

1. Pastikan Apache sudah running di XAMPP
2. Buka browser
3. Kunjungi: `http://localhost/tiket-app/api/`

Seharusnya melihat JSON:
```json
{
  "status": 200,
  "data": {
    "message": "Tiket App API is running",
    "version": "1.0"
  },
  "message": "API ready"
}
```

---

### 📝 IMPORTANT - Dalam 1 Jam

#### Step 4: Update React Context

File yang perlu diupdate: `src/context/AppContext.jsx`

Tambahkan import di atas:
```javascript
import { movieAPI, scheduleAPI, bookingAPI } from '../utils/apiService';
```

Update movies loading:
```javascript
// Sebelumnya:
const [movies, setMovies] = useState(() => {
  const stored = localStorage.getItem('movies');
  return stored ? JSON.parse(stored) : initialMovies;
});

// Sesudah:
const [movies, setMovies] = useState([]);

useEffect(() => {
  const loadMovies = async () => {
    try {
      const data = await movieAPI.getAll();
      setMovies(data);
    } catch (error) {
      console.error('Failed to load movies:', error);
    }
  };
  loadMovies();
}, []);
```

Lakukan hal yang sama untuk `schedules` dan `bookings`.

#### Step 5: Test React Integration

1. Buka VS Code terminal
2. Jalankan: `npm run dev`
3. Buka aplikasi di browser
4. Buka Developer Tools (F12) → Console
5. Seharusnya tidak ada error

---

### 🚀 NEXT - Dalam 1 Hari

#### Step 6: Update Components untuk API

Update setiap component yang menggunakan data:
- `AdminPanel.jsx` - untuk create/update/delete
- `MovieCard.jsx` - untuk display movies
- `SeatSelection.jsx` - untuk booking
- Dan lainnya

#### Step 7: Add Error Handling

Tambahkan try-catch di setiap API call:
```javascript
try {
  const data = await movieAPI.getAll();
  setMovies(data);
} catch (error) {
  console.error('Error loading movies:', error);
  // Show error message to user
}
```

#### Step 8: Test Semua Fitur

- [ ] Load movies dari database
- [ ] Load schedules
- [ ] Create booking
- [ ] Get user bookings
- [ ] Update booking status

---

## 🔗 API Endpoints yang Sudah Ready

### Movies
- ✅ `GET /api/movies` - Get all movies
- ✅ `GET /api/movies?id=1` - Get single movie
- ✅ `POST /api/movies` - Create movie
- ✅ `PUT /api/movies?id=1` - Update movie
- ✅ `DELETE /api/movies?id=1` - Delete movie

### Schedules
- ✅ `GET /api/schedules` - Get all schedules
- ✅ `GET /api/schedules?id=1` - Get single schedule
- ✅ `GET /api/schedules?movieId=1` - Get by movie
- ✅ `POST /api/schedules` - Create schedule
- ✅ `PUT /api/schedules?id=1` - Update schedule
- ✅ `DELETE /api/schedules?id=1` - Delete schedule

### Bookings
- ✅ `GET /api/bookings` - Get all bookings
- ✅ `GET /api/bookings?id=1` - Get single booking
- ✅ `GET /api/bookings?userId=123` - Get user bookings
- ✅ `POST /api/bookings` - Create booking
- ✅ `PUT /api/bookings?id=1` - Update booking status
- ✅ `DELETE /api/bookings?id=1` - Delete booking

---

## 📊 Database Schema

### Movies Table
```sql
id, title, genre, rating, duration, description, posterUrl, releaseDate, createdAt, updatedAt
```

### Schedules Table
```sql
id, movieId, time, date, price, createdAt, updatedAt
```

### Bookings Table
```sql
id, userId, scheduleId, seats (JSON), snacks (JSON), totalPrice, status, createdAt, updatedAt
```

### Transactions Table
```sql
id, bookingId, userId, amount, method, status, referenceNo, createdAt, updatedAt
```

---

## ⚠️ Important Notes

1. **Database Name**: Harus persis `tiket_app`
2. **API Base URL**: Default `http://localhost/tiket-app/api/`
3. **CORS**: Sudah di-handle, tapi jika error cek di `db_connect.php`
4. **Port Apache**: Default port 80
5. **React Dev Server**: Default port 5173

---

## 🐛 Troubleshooting Quick Guide

| Masalah | Solusi |
|--------|--------|
| API returns 404 | Pastikan folder `api` di htdocs, Apache running |
| Database connection error | Cek MySQL running, username/password di db_connect.php |
| CORS error di console | Hard refresh browser (Ctrl+F5) |
| Movies tidak muncul | Cek apakah data ada di database, cek console |
| POST/PUT tidak bekerja | Cek request body format, pastikan JSON valid |

---

## 📚 Reference Files

Baca file-file ini untuk informasi lengkap:
1. **API_SETUP_GUIDE.md** - Setup detail step-by-step
2. **API_QUICK_REFERENCE.md** - Quick reference & cheatsheet
3. **api/db_connect.php** - Konfigurasi database
4. **src/utils/apiService.js** - API service documentation

---

## ✨ Folder Structure Sekarang

```
tiket-app/
├── 📁 api/
│   ├── db_connect.php
│   ├── config.php
│   ├── index.php
│   ├── .htaccess
│   ├── tiket_app.sql
│   ├── 📁 models/
│   │   ├── MovieModel.php
│   │   ├── BookingModel.php
│   │   └── ScheduleModel.php
│   └── 📁 routes/
│       ├── movies.php
│       ├── schedules.php
│       └── bookings.php
│
├── 📁 src/
│   ├── 📁 components/
│   ├── 📁 context/
│   │   └── AppContext.jsx (PERLU DIUPDATE)
│   └── 📁 utils/
│       ├── apiService.js ✨ NEW
│       └── helpers.js
│
├── API_SETUP_GUIDE.md ✨ NEW
├── API_QUICK_REFERENCE.md ✨ NEW
└── SETUP_SUMMARY.md ✨ NEW
```

---

## 🎉 Done!

Semua file sudah siap. Tinggal:
1. ✅ Import database
2. ✅ Test API
3. ✅ Update React Context
4. ✅ Test React integration

**Estimasi waktu total: 1-2 jam**

---

**Created**: March 4, 2026
**Status**: Ready for Integration
**Next Step**: Import Database & Test API
