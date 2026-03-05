# 🎯 Visual Guide - Integrasi MySQL & PHP API

## 📊 Arsitektur Aplikasi

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│                    🌐 REACT FRONTEND                            │
│                   (Port 5173)                                   │
│                                                                  │
│   ┌────────────────────────────────────────────────────────┐   │
│   │  Components (MovieCard, SeatSelection, etc)            │   │
│   │         ↓                                              │   │
│   │  AppContext.jsx                                        │   │
│   │         ↓                                              │   │
│   │  src/utils/apiService.js  ← BARU! ✨                 │   │
│   │  (movieAPI, scheduleAPI, bookingAPI)                 │   │
│   └────────────────────────────────────────────────────────┘   │
│                         ↓                                        │
│                    HTTP Request                                 │
│                   (fetch API)                                   │
└─────────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│               🔌 PHP API BACKEND                                │
│              (Port 80 - Apache)                                 │
│                                                                  │
│   /api/index.php  (Router Utama)                              │
│         ↓                                                       │
│   /api/routes/                                                │
│   ├── movies.php                                             │
│   ├── schedules.php                                          │
│   └── bookings.php                                           │
│         ↓                                                       │
│   /api/models/                                               │
│   ├── MovieModel.php                                         │
│   ├── ScheduleModel.php                                      │
│   └── BookingModel.php                                       │
│         ↓                                                       │
│   /api/db_connect.php  (MySQL Connection)                     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│             🗄️  MYSQL DATABASE                                 │
│           (tiket_app database)                                 │
│                                                                  │
│   Tables:                                                       │
│   ├── movies (film data)                                       │
│   ├── schedules (jadwal film)                                  │
│   ├── bookings (pemesanan tiket)                               │
│   ├── transactions (transaksi pembayaran)                      │
│   └── users (data pengguna)                                    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔄 Data Flow

### Scenario 1: Load Movies

```
React Component
    ↓
import { movieAPI } from 'utils/apiService.js'
    ↓
movieAPI.getAll()
    ↓
fetch('http://localhost/tiket-app/api/movies')
    ↓
API Router (index.php) melihat 'movies'
    ↓
Include routes/movies.php
    ↓
GET request → new MovieModel($conn)->getAll()
    ↓
SELECT * FROM movies
    ↓
Return JSON data
    ↓
React setState(movies)
    ↓
UI Re-render dengan data dari database
```

### Scenario 2: Create Booking

```
User klik "Pesan Tiket"
    ↓
Component call bookingAPI.create(bookingData)
    ↓
POST to http://localhost/tiket-app/api/bookings
    ↓
Body:
{
  userId: "user123",
  scheduleId: 1,
  seats: ["A1", "A2"],
  totalPrice: 100000
}
    ↓
API Router parse 'bookings'
    ↓
Include routes/bookings.php
    ↓
POST request → new BookingModel($conn)->create(data)
    ↓
INSERT INTO bookings (...)
    ↓
Return JSON { id: 123, success: true }
    ↓
React update state
    ↓
Show success message & save ticket ID
```

---

## 📋 Action Checklist

### DO THIS NOW (⚡ 10 MINUTES)

- [ ] **Step 1: Open MySQL Command Line or phpMyAdmin**
  - Command Line: `mysql -u root -p`
  - Or: Open browser → `http://localhost/phpmyadmin`

- [ ] **Step 2: Import Database**
  - Command: `mysql -u root -p < C:\xampp\htdocs\tiket-app\api\tiket_app.sql`
  - Or: In phpMyAdmin → Import → Choose file → Import

- [ ] **Step 3: Verify Tables Created**
  - Run: `USE tiket_app; SHOW TABLES;`
  - Should see: movies, schedules, bookings, transactions, users

- [ ] **Step 4: Ensure Apache Running**
  - XAMPP Control Panel → Apache → START
  - Status bar should be GREEN ✓

- [ ] **Step 5: Test API**
  - Open browser → `http://localhost/tiket-app/api/`
  - Should see JSON response ✓

### DO THIS NEXT (📝 30 MINUTES)

- [ ] Open `src/context/AppContext.jsx`
- [ ] Add import: `import { movieAPI, scheduleAPI, bookingAPI } from '../utils/apiService';`
- [ ] Replace localStorage data fetch with API calls
- [ ] Test in browser - should load movies from database

### DO THIS LATER (🚀 1 HOUR)

- [ ] Update all components to use API
- [ ] Add error handling
- [ ] Add loading states
- [ ] Test create booking
- [ ] Test get user bookings

---

## 🧪 Quick API Test

### Using Browser (Simplest)

```
GET - Get all movies:
http://localhost/tiket-app/api/movies

GET - Get single movie:
http://localhost/tiket-app/api/movies?id=1

GET - Get schedules:
http://localhost/tiket-app/api/schedules

GET - Get schedules for movie 1:
http://localhost/tiket-app/api/schedules?movieId=1
```

### Using cURL (Advanced)

```bash
# Get all movies
curl http://localhost/tiket-app/api/movies

# Get movies with ID
curl http://localhost/tiket-app/api/movies?id=1

# Create booking
curl -X POST http://localhost/tiket-app/api/bookings \
  -H "Content-Type: application/json" \
  -d '{
    "userId": "test-user",
    "scheduleId": 1,
    "seats": ["A1", "A2"],
    "totalPrice": 90000
  }'
```

### Using Postman/Thunder Client (Best)

1. Download **Postman** atau **Thunder Client** (VS Code extension)
2. Create new request
3. Select `GET` method
4. Enter URL: `http://localhost/tiket-app/api/movies`
5. Click **Send**
6. Should see JSON response with movies

---

## 🗂️ File Organization

```
tiket-app/
│
├─ 🔵 BARU - Backend (PHP & Database)
│
│  api/
│  ├── .htaccess ✨ NEW
│  ├── db_connect.php ✨ NEW
│  ├── config.php ✨ NEW
│  ├── index.php ✨ NEW
│  ├── tiket_app.sql ✨ NEW
│  │
│  ├── models/ ✨ NEW
│  │  ├── MovieModel.php
│  │  ├── ScheduleModel.php
│  │  └── BookingModel.php
│  │
│  └── routes/ ✨ NEW
│     ├── movies.php
│     ├── schedules.php
│     └── bookings.php
│
├─ 🟢 UPDATED - Frontend (React)
│
│  src/
│  ├── utils/
│  │  └── apiService.js ✨ NEW
│  │     (movieAPI, scheduleAPI, bookingAPI)
│  │
│  ├── context/
│  │  └── AppContext.jsx ⚠️ NEEDS UPDATE
│  │     (Replace localStorage with API)
│  │
│  └── components/
│     ├── MovieCard.jsx ⚠️ NEEDS UPDATE
│     ├── SeatSelection.jsx ⚠️ NEEDS UPDATE
│     ├── TicketDetailModal.jsx ⚠️ NEEDS UPDATE
│     └── AdminPanel.jsx ⚠️ NEEDS UPDATE
│
└─ 📖 Documentation
   ├── API_SETUP_GUIDE.md ✨ NEW
   ├── API_QUICK_REFERENCE.md ✨ NEW
   └── SETUP_SUMMARY.md ✨ NEW
```

---

## ✅ Success Indicators

### ✓ Database Setup OK jika:
- Bisa login ke MySQL
- Database `tiket_app` ada
- Tabel `movies`, `schedules`, `bookings` ada
- Tabel berisi sample data (3 movies, 6 schedules)

### ✓ API Working OK jika:
- `http://localhost/tiket-app/api/` returns JSON
- `http://localhost/tiket-app/api/movies` shows list of movies
- Movies data match with database data
- No 404 or connection error

### ✓ React Integration OK jika:
- React app loads without error
- Movies display on screen
- Data comes from API (not hardcoded)
- Opening Dev Tools Console shows no fetch errors

---

## 🆘 Troubleshooting

### ❌ "Database Connection Failed"
```
Solution:
1. Check MySQL running (XAMPP)
2. Verify credentials in api/db_connect.php
3. Database name must be exactly "tiket_app"
4. User = "root", Password = "" (empty)
```

### ❌ "404 Not Found at /api/"
```
Solution:
1. Verify folder structure: 
   C:\xampp\htdocs\tiket-app\api\ exists
2. Apache must be running (green in XAMPP)
3. Check DocumentRoot in Apache config
4. Restart Apache
```

### ❌ "CORS Error in Browser Console"
```
Solution:
1. Hard refresh (Ctrl+F5)
2. Check db_connect.php has CORS headers
3. Verify API_BASE_URL in apiService.js is correct
4. Try from different browser
```

### ❌ "Module not found: apiService"
```
Solution:
1. Verify file exists:
   src/utils/apiService.js
2. Check import path is correct:
   import { movieAPI } from '../utils/apiService'
3. Check file name spelling (case sensitive)
```

### ❌ "No data showing in React"
```
Solution:
1. Open DevTools (F12) → Network tab
2. Check if fetch request is made to API
3. Check response status (200 OK?)
4. Check response body has data
5. Check console for JavaScript errors
```

---

## 📞 Quick Help

### Need to Reset Database?
```sql
DROP DATABASE tiket_app;
-- Then re-import tiket_app.sql
```

### Need to Add New Field to Movie?
```sql
ALTER TABLE movies ADD COLUMN newField VARCHAR(255);
-- Then update MovieModel.php
```

### Need to Test Specific Endpoint?
```bash
# Use cURL or Postman
# See examples above in "Quick API Test"
```

---

## 🚀 You're Ready!

All backend infrastructure is ready:
- ✅ Database schema created
- ✅ PHP API routes set up
- ✅ Models and database connection ready
- ✅ React API service created
- ✅ Documentation provided

**Now you just need to:**
1. Import the database
2. Test the API
3. Update React Context
4. Update Components

**Estimated time: 1-2 hours**

---

**Status**: ✨ READY FOR INTEGRATION
**Last Updated**: March 4, 2026
**Version**: 1.0
