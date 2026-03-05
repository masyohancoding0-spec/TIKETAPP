# Quick Reference - Tiket App API Integration

## 🚀 Quick Start (5 menit)

### 1. Setup Database
```bash
# Buka MySQL Command Line
mysql -u root -p
# Kemudian import SQL file:
mysql -u root -p < C:\xampp\htdocs\tiket-app\api\tiket_app.sql
```

### 2. Start Apache
- Buka XAMPP Control Panel → Click START di Apache

### 3. Test API
```
http://localhost/tiket-app/api/
```

Harus muncul JSON response ✓

---

## 📡 API Endpoints Cheatsheet

### Get All Movies
```javascript
const movies = await movieAPI.getAll();
```

### Get Movie by ID
```javascript
const movie = await movieAPI.getById(1);
```

### Get Schedules by Movie
```javascript
const schedules = await scheduleAPI.getByMovieId(1);
```

### Create Booking
```javascript
const booking = await bookingAPI.create({
  userId: 'user123',
  scheduleId: 1,
  seats: ['A1', 'A2'],
  snacks: [{ id: 1, name: 'Popcorn' }],
  totalPrice: 100000
});
```

### Get User Bookings
```javascript
const bookings = await bookingAPI.getByUserId('user123');
```

### Update Booking Status
```javascript
await bookingAPI.updateStatus(bookingId, 'confirmed');
```

---

## 🗂️ File Structure

```
tiket-app/
├── api/                          # 📁 Backend PHP
│   ├── index.php                 # 🎯 Main router
│   ├── db_connect.php            # 🔗 Database connection
│   ├── config.php                # ⚙️ Configuration
│   ├── tiket_app.sql             # 🗄️ Database schema
│   ├── models/                   # 📊 Data models
│   └── routes/                   # 🛣️ API routes
│
└── src/
    └── utils/
        └── apiService.js         # 🔌 React API client
```

---

## 🔧 Configuration

**Database Settings** (`api/db_connect.php`)
```php
define('DB_HOST', 'localhost');     // Biasanya localhost
define('DB_USER', 'root');          // Username MySQL
define('DB_PASS', '');              // Password (kosong untuk default XAMPP)
define('DB_NAME', 'tiket_app');     // Nama database
```

**API Base URL** (`src/utils/apiService.js`)
```javascript
const API_BASE_URL = 'http://localhost/tiket-app/api';
```

---

## 🧪 Testing dengan cURL

```bash
# Get all movies
curl http://localhost/tiket-app/api/movies

# Get movie by ID
curl http://localhost/tiket-app/api/movies?id=1

# Get schedules by movie
curl http://localhost/tiket-app/api/schedules?movieId=1

# Create booking
curl -X POST http://localhost/tiket-app/api/bookings \
  -H "Content-Type: application/json" \
  -d '{
    "userId": "user123",
    "scheduleId": 1,
    "seats": ["A1", "A2"],
    "totalPrice": 100000
  }'
```

---

## 🐛 Common Issues & Solutions

| Issue | Cause | Solution |
|-------|-------|----------|
| API returns 404 | Wrong endpoint | Check URL di browser |
| Database connection error | MySQL not running | Start MySQL di XAMPP |
| CORS error | Browser blocking request | Check CORS headers di db_connect.php |
| Module not found (apiService) | Wrong import path | Check import di React |
| Data tidak update | Cache issue | Hard refresh browser (Ctrl+F5) |

---

## 📋 Response Format

### Success Response (200)
```json
{
  "status": 200,
  "data": [
    {
      "id": 1,
      "title": "Gundala",
      "genre": "Action",
      ...
    }
  ],
  "message": "Movies retrieved successfully"
}
```

### Error Response (400/500)
```json
{
  "status": 400,
  "error": "Invalid request data"
}
```

---

## 🔐 Security Notes

1. **Jangan hard-code credentials** di client-side
2. **Validasi semua input** di backend sebelum execute query
3. **Gunakan prepared statements** untuk prevent SQL injection (sudah diimplementasikan)
4. **Add authentication** sebelum production
5. **Hash passwords** jika ada user management

---

## 🚀 Deployment Checklist

- [ ] Database sudah backup
- [ ] API sudah tested di localhost
- [ ] React app sudah build (`npm run build`)
- [ ] API Base URL di-update ke production server
- [ ] Database credentials updated
- [ ] CORS settings di-adjust untuk production domain
- [ ] Error handling sudah di-add
- [ ] Logging sudah di-implement

---

## 📚 Additional Resources

- **Full Setup Guide**: `API_SETUP_GUIDE.md`
- **Database Schema**: `api/tiket_app.sql`
- **API Service**: `src/utils/apiService.js`
- **Context**: `src/context/AppContext.jsx`

---

**Last Updated**: March 4, 2026
**Version**: 1.0
