# Panduan Setup API MySQL dan React Integration

## Ringkasan
Aplikasi Tiket sudah diintegrasikan dengan MySQL Database dan PHP API lokal. Struktur project sudah siap, tinggal melakukan beberapa setup di MySQL dan React.

## Struktur Project

```
tiket-app/
├── api/                          # Folder PHP API
│   ├── db_connect.php           # Koneksi database MySQL
│   ├── config.php               # Konfigurasi API
│   ├── index.php                # Router API utama
│   ├── tiket_app.sql            # Schema database
│   ├── models/
│   │   ├── MovieModel.php       # Model untuk film
│   │   ├── BookingModel.php     # Model untuk pemesanan
│   │   └── ScheduleModel.php    # Model untuk jadwal
│   └── routes/
│       ├── movies.php           # Endpoint film
│       ├── schedules.php        # Endpoint jadwal
│       └── bookings.php         # Endpoint pemesanan
│
├── src/
│   └── utils/
│       └── apiService.js        # Service untuk API call dari React
```

---

## BAGIAN 1: Setup Database MySQL

### Step 1: Buka MySQL Command Line atau phpMyAdmin

**Menggunakan Command Line:**
```bash
mysql -u root -p
```

Jika tidak ada password, cukup tekan Enter.

**Atau menggunakan phpMyAdmin:**
- Buka browser: http://localhost/phpmyadmin
- Login dengan username: `root` (password kosong)

### Step 2: Import Database Schema

**Menggunakan Command Line:**
```bash
mysql -u root -p < C:\xampp\htdocs\tiket-app\api\tiket_app.sql
```

**Menggunakan phpMyAdmin:**
1. Di phpMyAdmin, klik tab **"Import"**
2. Click **"Choose File"** dan pilih file `C:\xampp\htdocs\tiket-app\api\tiket_app.sql`
3. Scroll ke bawah dan click **"Import"**

### Step 3: Verifikasi Database

Di MySQL Command Line, jalankan:
```sql
USE tiket_app;
SHOW TABLES;
```

Anda seharusnya melihat tabel: `movies`, `schedules`, `bookings`, `transactions`, `users`

---

## BAGIAN 2: Setup Apache untuk PHP API

### Step 1: Pastikan Apache sudah berjalan
- Buka XAMPP Control Panel
- Pastikan **Apache** sudah start (tombol berwarna hijau)

### Step 2: Setup Virtual Host (Opsional tapi Recommended)

Untuk mengakses API dengan URL yang lebih clean, edit virtual host:

**File:** `C:\xampp\apache\conf\extra\httpd-vhosts.conf`

Tambahkan di akhir file:
```apache
<VirtualHost *:80>
    ServerName localhost
    DocumentRoot "C:/xampp/htdocs"
</VirtualHost>

<VirtualHost *:80>
    ServerName tiket-app.local
    DocumentRoot "C:/xampp/htdocs/tiket-app"
</VirtualHost>
```

Tambahkan ke host file: `C:\Windows\System32\drivers\etc\hosts`
```
127.0.0.1 tiket-app.local
```

Restart Apache di XAMPP Control Panel.

### Step 3: Test API

Buka browser dan test:
```
http://localhost/tiket-app/api/
```

Anda seharusnya melihat response JSON:
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

## BAGIAN 3: Update React Context untuk Menggunakan API

Saat ini, React masih menggunakan localStorage. Kita akan mengupdate AppContext untuk menggunakan API.

### File yang perlu diupdate:
`src/context/AppContext.jsx`

**Contoh implementasi:**

```javascript
import { movieAPI, scheduleAPI, bookingAPI } from '../utils/apiService';

export function AppProvider({ children }) {
  const [movies, setMovies] = useState([]);
  const [schedules, setSchedules] = useState([]);
  const [loading, setLoading] = useState(true);

  // Load movies dari API
  useEffect(() => {
    const loadMovies = async () => {
      try {
        const data = await movieAPI.getAll();
        setMovies(data);
      } catch (error) {
        console.error('Failed to load movies:', error);
        // Fallback ke localStorage jika API error
      } finally {
        setLoading(false);
      }
    };

    loadMovies();
  }, []);

  // Load schedules dari API
  useEffect(() => {
    const loadSchedules = async () => {
      try {
        const data = await scheduleAPI.getAll();
        setSchedules(data);
      } catch (error) {
        console.error('Failed to load schedules:', error);
      }
    };

    loadSchedules();
  }, []);

  // ... rest of context
}
```

---

## BAGIAN 4: API Endpoints Reference

### Movies
- `GET /api/movies` - Get semua film
- `GET /api/movies?id=1` - Get film by ID
- `POST /api/movies` - Create film (admin)
- `PUT /api/movies?id=1` - Update film (admin)
- `DELETE /api/movies?id=1` - Delete film (admin)

### Schedules
- `GET /api/schedules` - Get semua jadwal
- `GET /api/schedules?id=1` - Get jadwal by ID
- `GET /api/schedules?movieId=1` - Get jadwal by film
- `POST /api/schedules` - Create jadwal
- `PUT /api/schedules?id=1` - Update jadwal
- `DELETE /api/schedules?id=1` - Delete jadwal

### Bookings
- `GET /api/bookings` - Get semua pemesanan
- `GET /api/bookings?id=1` - Get pemesanan by ID
- `GET /api/bookings?userId=user123` - Get user's bookings
- `POST /api/bookings` - Create pemesanan
- `PUT /api/bookings?id=1` - Update status pemesanan
- `DELETE /api/bookings?id=1` - Delete pemesanan

---

## BAGIAN 5: Testing API dengan Postman/Thunder Client

### Test GET Movies:
```
GET http://localhost/tiket-app/api/movies
```

### Test POST Booking:
```
POST http://localhost/tiket-app/api/bookings
Content-Type: application/json

{
  "userId": "user123",
  "scheduleId": 1,
  "seats": ["A1", "A2"],
  "snacks": [{"id": 1, "name": "Popcorn Regular"}],
  "totalPrice": 100000
}
```

---

## BAGIAN 6: Troubleshooting

### API returns "Database Connection Error"
- Pastikan MySQL service berjalan
- Check username dan password di `api/db_connect.php`
- Database name harus `tiket_app`

### CORS Error
- CORS sudah di-handle di `db_connect.php`
- Pastikan React running di port yang benar

### 404 Not Found di API
- Pastikan folder `api` berada di root htdocs
- Check Apache DocumentRoot menunjuk ke folder yang benar

### Module not found di React (apiService)
- Pastikan file `src/utils/apiService.js` sudah dibuat
- Pastikan import path benar

---

## BAGIAN 7: Langkah Berikutnya

1. **Update AppContext.jsx** untuk menggunakan API
2. **Update components** yang membuat data baru atau update data
3. **Add Authentication** untuk user management
4. **Add Error Handling** di React components
5. **Deploy ke production** (server dengan PHP dan MySQL)

---

## Quick Start Checklist

- [ ] MySQL database sudah dibuat dengan nama `tiket_app`
- [ ] Semua tabel sudah ter-import (movies, schedules, bookings, transactions, users)
- [ ] Apache/PHP berjalan di `http://localhost`
- [ ] Folder `api` berada di `htdocs/tiket-app/api/`
- [ ] Bisa akses API di `http://localhost/tiket-app/api/`
- [ ] File `apiService.js` sudah dibuat di `src/utils/`
- [ ] React sudah mengimport dari `apiService.js`

---

## Support Files

- Database Schema: `api/tiket_app.sql`
- PHP Database Connection: `api/db_connect.php`
- API Service: `src/utils/apiService.js`
- Documentation: `API_SETUP_GUIDE.md` (file ini)
