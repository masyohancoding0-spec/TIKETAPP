# 📝 STEP-BY-STEP IMPLEMENTATION GUIDE

## ⏱️ Total Time: ~2 Hours

---

## 🎯 PHASE 1: DATABASE SETUP (⏱️ 10 MINUTES)

### Step 1.1: Open MySQL Connection

**Option A - Command Line (Windows):**
```bash
# Open Command Prompt or PowerShell
# Type:
mysql -u root -p

# If XAMPP not in PATH, navigate to:
cd C:\xampp\mysql\bin
mysql -u root -p

# Press Enter when asked for password (it's empty in XAMPP)
```

**Option B - phpMyAdmin GUI (Easier):**
```
1. Start Apache in XAMPP Control Panel
2. Open browser: http://localhost/phpmyadmin
3. Login with username "root" (no password)
```

### Step 1.2: Import Database Schema

**Using Command Line:**
```bash
# Copy this command and run:
mysql -u root -p < C:\xampp\htdocs\tiket-app\api\tiket_app.sql

# Press Enter when asked for password
```

**Using phpMyAdmin:**
```
1. In phpMyAdmin, click "Import" tab
2. Click "Choose File"
3. Select: C:\xampp\htdocs\tiket-app\api\tiket_app.sql
4. Scroll down and click "Import" button
5. Wait for completion message
```

### Step 1.3: Verify Database Created

Run these commands in MySQL:

```sql
-- Check if database exists
SHOW DATABASES;
-- Should see: tiket_app

-- Select database
USE tiket_app;

-- Check tables
SHOW TABLES;
-- Should see 5 tables:
-- 1. bookings
-- 2. movies
-- 3. schedules
-- 4. transactions
-- 5. users

-- Check movies data
SELECT * FROM movies;
-- Should see 3 movies

-- Check schedules
SELECT * FROM schedules;
-- Should see 6 schedules
```

✅ **If you see the data, database setup is COMPLETE!**

---

## 🔌 PHASE 2: API VERIFICATION (⏱️ 5 MINUTES)

### Step 2.1: Ensure Apache is Running

```
1. Open XAMPP Control Panel
2. Look for "Apache" row
3. Button should be "Stop" (meaning it's running)
4. If showing "Start", click it to start
```

### Step 2.2: Test API Endpoint

```
1. Open your browser
2. Go to: http://localhost/tiket-app/api/

3. You should see this JSON response:
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

### Step 2.3: Test Movies Endpoint

```
1. In same browser, go to:
   http://localhost/tiket-app/api/movies

2. You should see JSON array with 3 movies:
```json
[
  {
    "id": 1,
    "title": "Gundala: Putra Petir",
    "genre": "Action, Superhero",
    "rating": 4.8,
    ...
  },
  ...
]
```

✅ **If you see the data, API is WORKING!**

---

## ⚛️ PHASE 3: REACT INTEGRATION (⏱️ 45 MINUTES)

### Step 3.1: Verify apiService.js Exists

```bash
# In VS Code, check if this file exists:
src/utils/apiService.js

# Should contain:
# - movieAPI.getAll()
# - movieAPI.getById()
# - scheduleAPI.*
# - bookingAPI.*
```

### Step 3.2: Update AppContext.jsx

**File Location:** `src/context/AppContext.jsx`

**Find this section (line ~70):**
```javascript
const [movies, setMovies] = useState(() => {
  const stored = localStorage.getItem('movies');
  return stored ? JSON.parse(stored) : initialMovies;
});
```

**Replace with:**
```javascript
const [movies, setMovies] = useState([]);

// Load movies from API
useEffect(() => {
  const loadMovies = async () => {
    try {
      const data = await movieAPI.getAll();
      if (data) {
        setMovies(data);
      }
    } catch (error) {
      console.error('Failed to load movies:', error);
      // Fallback to initial data if API fails
      setMovies(initialMovies);
    }
  };

  loadMovies();
}, []);
```

**Add this import at the top (line 1):**
```javascript
import { movieAPI, scheduleAPI, bookingAPI } from '../utils/apiService';
```

### Step 3.3: Do the Same for Schedules

**Find this section:**
```javascript
const [schedules, setSchedules] = useState(() => {
  const stored = localStorage.getItem('schedules');
  return stored ? JSON.parse(stored) : initialSchedules;
});
```

**Replace with:**
```javascript
const [schedules, setSchedules] = useState([]);

// Load schedules from API
useEffect(() => {
  const loadSchedules = async () => {
    try {
      const data = await scheduleAPI.getAll();
      if (data) {
        setSchedules(data);
      }
    } catch (error) {
      console.error('Failed to load schedules:', error);
      setSchedules(initialSchedules);
    }
  };

  loadSchedules();
}, []);
```

### Step 3.4: Update Bookings (Similar to above)

**Find the bookings state and replace similarly**

### Step 3.5: Update Create Booking Function

**Find the function that creates booking (search for "createBooking")**

**Change from:**
```javascript
const createBooking = () => {
  // ... logic ...
  setBookings([...bookings, newBooking]);
}
```

**To:**
```javascript
const createBooking = async (bookingData) => {
  try {
    const result = await bookingAPI.create(bookingData);
    
    // Update local state
    setBookings([...bookings, result]);
    
    // Show success
    console.log('Booking created:', result);
    return result;
  } catch (error) {
    console.error('Failed to create booking:', error);
    throw error;
  }
}
```

### Step 3.6: Test React App

```bash
1. Open VS Code terminal
2. Run: npm run dev
3. In browser, go to: http://localhost:5173
4. Open DevTools (F12) → Network tab
5. Watch for API requests to:
   http://localhost/tiket-app/api/movies
6. Should see movies from database
7. Check Console tab for errors
```

✅ **If movies show from database, React integration is WORKING!**

---

## 📦 PHASE 4: COMPONENTS UPDATE (⏱️ 45 MINUTES)

### Step 4.1: Update MovieCard Component

**File:** `src/components/MovieCard.jsx`

**Ensure it displays data from API** (should work automatically after Context update)

### Step 4.2: Update SeatSelection Component

**File:** `src/components/SeatSelection.jsx`

**When booking is submitted, call:**
```javascript
const handleBooking = async () => {
  try {
    await bookingAPI.create({
      userId: currentUser.id,
      scheduleId: selectedSchedule.id,
      seats: selectedSeats,
      snacks: selectedSnacks,
      totalPrice: totalPrice
    });
    
    // Show success message
    // Redirect to booking confirmation
  } catch (error) {
    // Show error message
  }
}
```

### Step 4.3: Update TicketDetailModal

**File:** `src/components/TicketDetailModal.jsx`

**When displaying ticket, fetch from API:**
```javascript
useEffect(() => {
  if (bookingId) {
    bookingAPI.getById(bookingId)
      .then(data => setBooking(data))
      .catch(error => console.error(error));
  }
}, [bookingId]);
```

### Step 4.4: Update AdminPanel (If Needed)

**File:** `src/components/AdminPanel.jsx`

**Add functions to:**
- Create movie: `movieAPI.create(data)`
- Update movie: `movieAPI.update(id, data)`
- Delete movie: `movieAPI.delete(id)`
- Create schedule: `scheduleAPI.create(data)`
- Delete schedule: `scheduleAPI.delete(id)`

---

## 🧪 PHASE 5: TESTING (⏱️ 15 MINUTES)

### Test 5.1: Load Movies

```
✓ Click on "Home" page
✓ Movies should load from database
✓ Check DevTools Network tab → movies request
✓ Check response shows 3 movies
```

### Test 5.2: View Movie Details

```
✓ Click on a movie
✓ Should show full details from database
✓ Check Console for any errors
```

### Test 5.3: Load Schedules

```
✓ Click "Booking" section
✓ Schedules should load for selected movie
✓ Check response shows schedules
```

### Test 5.4: Create Booking

```
✓ Select seats
✓ Select snacks
✓ Click "Book"
✓ Check if booking is saved to database
✓ Verify in MySQL: SELECT * FROM bookings;
```

### Test 5.5: Get User Bookings

```
✓ Go to "My Tickets"
✓ Should show bookings from database
✓ API call: bookingAPI.getByUserId(userId)
```

---

## ✅ PHASE 6: FINAL CHECKLIST

### Database ✓
- [ ] MySQL running
- [ ] Database `tiket_app` exists
- [ ] All 5 tables created with data
- [ ] Sample data visible (3 movies, 6 schedules)

### Backend API ✓
- [ ] Apache running (XAMPP)
- [ ] Can access `http://localhost/tiket-app/api/`
- [ ] Can access `http://localhost/tiket-app/api/movies`
- [ ] All 3 endpoints working (movies, schedules, bookings)

### React ✓
- [ ] `apiService.js` exists in `src/utils/`
- [ ] `AppContext.jsx` updated with API calls
- [ ] React app runs without errors: `npm run dev`
- [ ] Movies load from database (not hardcoded)
- [ ] Schedules load from API

### Features Working ✓
- [ ] Can view all movies from database
- [ ] Can view movie details
- [ ] Can view schedules for movie
- [ ] Can create booking (saves to database)
- [ ] Can view user's bookings

### Documentation ✓
- [ ] Read `API_SETUP_GUIDE.md`
- [ ] Read `API_QUICK_REFERENCE.md`
- [ ] Read `INTEGRATION_GUIDE.md`
- [ ] Saved this checklist

---

## 🆘 TROUBLESHOOTING QUICK FIXES

### Problem: MySQL Connection Error
```
Solution:
1. Stop Apache in XAMPP
2. Start MySQL service
3. Start Apache again
4. Verify connection in db_connect.php
```

### Problem: API returns 404
```
Solution:
1. Check folder structure: api/ exists
2. Check API URL: http://localhost/tiket-app/api/
3. Restart Apache
4. Check Apache error log: C:\xampp\apache\logs\
```

### Problem: React doesn't load data
```
Solution:
1. Open DevTools (F12)
2. Network tab → check if movies request succeeds
3. If fails, check API response
4. Check Console for JavaScript errors
5. Hard refresh (Ctrl+F5)
```

### Problem: CORS Error
```
Solution:
1. Check apiService.js has correct API_BASE_URL
2. Check db_connect.php has CORS headers
3. Clear browser cache
4. Restart Apache
```

---

## 📞 QUICK COMMAND REFERENCE

```bash
# Import database
mysql -u root -p < C:\xampp\htdocs\tiket-app\api\tiket_app.sql

# Connect to MySQL
mysql -u root -p

# In MySQL, check database
USE tiket_app;
SHOW TABLES;

# In React project, start dev server
npm run dev

# Build React for production
npm run build
```

---

## 🎉 YOU'RE ALL SET!

Once all checks are complete, your Tiket App is fully integrated with:
- ✅ MySQL Database
- ✅ PHP API Backend
- ✅ React Frontend

**Next Steps:**
1. Deploy to production server (with PHP & MySQL)
2. Setup proper authentication
3. Add payment gateway integration
4. Setup email notifications

---

**Created:** March 4, 2026
**Version:** 1.0
**Status:** Ready for Implementation
