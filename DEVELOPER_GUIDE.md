# 👨‍💻 DEVELOPER GUIDE - CinemaHub

## 📚 Struktur File & Penjelasan

### **src/AppNew.jsx** (Main App Component)
- **Size**: ~600 lines
- **Purpose**: Root component yang manage routing antar pages
- **Key State**: 
  - `activeTab` - Current page (home, booking, mytickets, admin)
  - `bookingStep` - Step dalam booking flow (seats, snacks, payment, confirmation)
  - `selectedMovie`, `selectedSchedule`, `selectedSeats`, `selectedSnacks`
- **Features**:
  - Login modal
  - Navigation header dengan mobile responsive
  - Page routing dengan conditional rendering
  - Background effects dengan gradient & shapes

### **src/context/AppContext.jsx** (Global State Management)
- **Size**: ~300 lines
- **Purpose**: Centralized state management menggunakan React Context
- **Key Features**:
  - Auth state (currentUser)
  - Movies & Schedules management
  - Seat locking mechanism dengan timeout
  - Booking & Transaction management
  - localStorage persistence untuk semua data
- **Hooks Provided**:
  ```javascript
  - currentUser, setCurrentUser
  - movies, schedules, snackOptions, paymentMethods
  - addMovie, updateMovie, deleteMovie
  - addSchedule, updateSchedule, deleteSchedule
  - lockSeat, unlockSeat, getBookedSeatsForSchedule
  - bookings, createBooking, confirmBooking
  - transactions, createTransaction, updateTransaction
  ```

### **src/hooks/useSeatManager.js** (Seat Management Logic)
- **Size**: ~150 lines
- **Purpose**: Custom hook untuk handle seat selection & locking
- **Key Functions**:
  ```javascript
  - generateSeatLayout() → [A1, A2, ..., E8]
  - getSeatStatus(seatCode) → 'available'|'booked'|'locked'|'selected'
  - toggleSeat(seatCode) → boolean
  - confirmSeats() → {seats, count}
  - clearSelectedSeats() → void
  ```
- **Race Condition Fix**:
  - Lock seat dengan unique lockId
  - Auto-unlock setelah 5 menit
  - Track locks per schedule

### **src/components/** (UI Components)

#### **1. MovieCard.jsx** (~140 lines)
- Display film poster, info, rating
- Show available schedules
- Quick booking button
- Hover zoom effect
- Glassmorphism styling

```javascript
<MovieCard 
  movie={movieObj} 
  schedules={schedulesForMovie} 
  onSelect={handleSelectMovie} 
/>
```

#### **2. SeatSelection.jsx** (~240 lines)
- Interactive 5x8 seat grid
- 3D perspective cinema screen
- Color-coded seat status
- Seat selection dengan animation
- Max seats validation
- Legend & booking summary

```javascript
<SeatSelection 
  scheduleId={selectedSchedule.id}
  onSeatsSelected={handleSeatsConfirmed}
/>
```

#### **3. SnackSelection.jsx** (~220 lines)
- Grid 3 kolom snack options
- Quantity controls (+ / -)
- Real-time price calculation
- Visual cart dengan selected items
- Price breakdown

```javascript
<SnackSelection 
  snacks={snackOptions}
  selectedSnacks={selectedSnacks}
  onSnacksChange={setSelectedSnacks}
/>
```

#### **4. PaymentGateway.jsx** (~280 lines)
- 3 payment method cards
- Provider selection per method
- Order summary
- Simulasi payment processing
- Error handling & success state

```javascript
<PaymentGateway
  amount={totalAmount}
  bookingId={bookingId}
  movieTitle={movieTitle}
  scheduleDateTime={scheduleDateTime}
  seats={selectedSeats}
  onPaymentSuccess={handlePaymentSuccess}
  onPaymentError={handlePaymentError}
/>
```

#### **5. TicketDisplay.jsx** (~290 lines)
- Success confirmation view
- Ticket preview dengan styling premium
- PDF download buttons (tiket + nota)
- Copy booking ID
- Detailed ticket info

```javascript
<TicketDisplay
  booking={bookingObj}
  movie={movieObj}
  schedule={scheduleObj}
  selectedSeats={seatsArray}
  snacks={snacksArray}
  transaction={transactionObj}
  onClose={handleClose}
/>
```

#### **6. AdminPanel.jsx** (~550 lines)
- Film & Schedule management tabs
- CRUD operations untuk film
- CRUD operations untuk jadwal
- Poster upload dengan validation
- Film list dengan action buttons
- Schedule list dengan inline edit

```javascript
<AdminPanel />
```

### **src/utils/helpers.js** (~150 lines)
- File validation
- Email validation
- Currency & date formatting
- Payment simulation
- Base64 file reading
- LocalStorage manager class
- Booking calculations

### **src/utils/pdfGenerator.js** (~200 lines)
- PDF generation menggunakan jsPDF + html2canvas
- Ticket PDF dengan styling
- Receipt PDF dengan breakdown
- Text-based PDF alternative

### **src/index.css** (~360 lines)
- Tailwind base imports
- Keyframe animations (shimmer, glow, etc)
- Glass morphism effects
- Golden styling & gradients
- Cinema screen 3D perspective
- Custom scrollbar
- Utility classes

### **src/main.jsx** (Entry point)
- React StrictMode
- AppProvider wrapper
- Root render

---

## 🔧 CUSTOMIZATION GUIDE

### **1. Mengubah Warna Theme**

#### **Gold Color:**
**File**: `src/index.css`
```css
:root {
  --golden: #FFD700;  /* Bright gold */
  --golden-dark: #D4AF37;  /* Dark gold */
}
```
**Change ke**: Warna pilihan Anda

#### **Background Gradient:**
**File**: `src/AppNew.jsx` (line ~100)
```jsx
background: linear-gradient(135deg, #0f0f0f 0%, #232526 100%),
```
**Change ke**: Custom gradient

#### **Component Colors:**
Button, card backgrounds, borders - ubah di Tailwind classes:
- `from-yellow-400 to-orange-500` → `from-blue-400 to-purple-500`
- `bg-slate-800` → `bg-gray-800`

### **2. Mengubah Jumlah Kursi**

**File**: `src/context/AppContext.jsx` (line ~60-70)
```javascript
const generateSeatLayout = () => {
  const rows = ['A', 'B', 'C', 'D', 'E'];  // 5 rows
  for (const row of rows) {
    for (let col = 1; col <= 8; col++) {  // 8 columns
      seats.push(`${row}${col}`);
    }
  }
  return seats;
};
```

**To change 5x8 ke 6x10:**
```javascript
const rows = ['A', 'B', 'C', 'D', 'E', 'F'];  // 6 rows
for (let col = 1; col <= 10; col++) {  // 10 columns
```

### **3. Mengubah Max Seats Per Booking**

**File**: `src/hooks/useSeatManager.js` (line ~15)
```javascript
const maxSeatsPerBooking = 5;  // Change ke 10, 8, dll
```

### **4. Menambah Snack/Minuman**

**File**: `src/context/AppContext.jsx` (line ~30-35)
```javascript
const snackOptions = [
  { id: 1, name: 'Popcorn Regular', price: 20000, icon: '🍿' },
  { id: 2, name: 'Popcorn Large', price: 35000, icon: '🍿' },
  // Add new item:
  { id: 7, name: 'Coffee Hot', price: 15000, icon: '☕' },
  // ...
];
```

### **5. Mengubah Proses Payment Timeout**

**File**: `src/utils/helpers.js` (line ~40-50)
```javascript
// Current: 2-4 seconds
setTimeout(() => {
  // ... paymentres
}, 2000 + Math.random() * 2000);  // Change 2000 & 2000 values
```

### **6. Mengubah Seat Lock Duration**

**File**: `src/context/AppContext.jsx` (line ~150-160)
```javascript
const timer = setTimeout(() => {
  // ... unlock
}, 5 * 60 * 1000);  // 5 minutes - change ke 10 * 60 * 1000 untuk 10 menit
```

### **7. Initial Sample Data**

**File**: `src/context/AppContext.jsx`

#### **Movies:**
```javascript
const initialMovies = [
  {
    id: 1,
    title: 'Your Movie Title',
    genre: 'Genre1, Genre2',
    duration: 120,  // Menit
    description: 'Movie description...',
    rating: 4.8,  // 0-5
    posterUrl: 'https://image-url.jpg',
    releaseDate: '2024-01-15',
  },
  // ...
];
```

#### **Schedules:**
```javascript
const initialSchedules = [
  {
    id: 1,
    movieId: 1,
    time: '14:00',  // HH:MM format
    date: '2024-12-15',  // YYYY-MM-DD format
    price: 45000,  // Rupiah
  },
  // ...
];
```

#### **Snacks:**
```javascript
const snackOptions = [
  {
    id: 1,
    name: 'Popcorn Regular',
    price: 20000,  // Rupiah
    icon: '🍿'
  },
  // ...
];
```

### **8. Authentication Custom**

**Current**: Simple name + email login

**To add backend auth:**
1. Create login API endpoint
2. Replace `setCurrentUser` logic dengan API call
3. Move token ke secure storage/cookies
4. Add JWT validation

---

## 🔌 INTEGRATING BACKEND API

### **Replace localStorage dengan API:**

**Example untuk getMovies:**

**Before (localStorage):**
```javascript
const [movies, setMovies] = useState(() => {
  const stored = localStorage.getItem('movies');
  return stored ? JSON.parse(stored) : initialMovies;
});
```

**After (API):**
```javascript
const [movies, setMovies] = useState([]);

useEffect(() => {
  fetch('/api/movies')
    .then(res => res.json())
    .then(data => setMovies(data))
    .catch(err => console.error(err));
}, []);
```

### **Add Real Payment Gateway:**

Replace `simulatePaymentProcessing` dengan:
```javascript
export async function processPaymentMidtrans(amount, data) {
  const response = await fetch('/api/payment/create', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      amount,
      user: data.email,
      orderId: data.bookingId,
    })
  });
  const result = await response.json();
  // Redirect ke Midtrans payment page
  window.location.href = result.paymentUrl;
}
```

### **Email Notification:**

Add setelah booking success:
```javascript
fetch('/api/email/send-ticket', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    userEmail: currentUser.email,
    bookingRef: booking.id,
    ticketPDF: ticketBase64,
  })
});
```

---

## 🧪 TESTING

### **Manual Testing Checklist:**

- [ ] Login/logout flow
- [ ] Search & filter movies
- [ ] Seat selection dengan semua status
- [ ] Max 5 seats validation
- [ ] Snack add/remove
- [ ] All 3 payment methods
- [ ] PDF download
- [ ] Admin add/edit/delete film
- [ ] Admin add/edit/delete schedule
- [ ] File upload validation
- [ ] Mobile responsiveness
- [ ] Dark theme visibility
- [ ] Animation smoothness

### **Common Issues to Check:**

```javascript
// 1. Seat not locking?
// Check useSeatManager.js lockSeat logic

// 2. PDF not generating?
// Check pdfGenerator.js untuk element IDs

// 3. Data not persisting?
// Check localStorage setters di AppContext

// 4. Payment not simulating?
// Check simulatePaymentProcessing timeout

// 5. Component not rendering?
// Check activeTab & bookingStep state
```

---

## 📝 CODE STYLE

### **Naming Conventions:**
- Components: `PascalCase` (MovieCard.jsx)
- Functions: `camelCase` (handleSelectMovie)
- Constants: `UPPER_SNAKE_CASE` (MAX_SEATS)
- Hooks: `useXxx` (useSeatManager)

### **Comments:**
```javascript
// Use untuk single line
// Ada space setelah //

/*
  Use untuk multi-line
  Code explanation
*/
```

### **Component Structure:**
```javascript
// 1. Imports
import React from 'react';

// 2. Constants
const MAX_SEATS = 5;

// 3. Component
export function MyComponent({ prop1, prop2 }) {
  // Hooks
  const [state, setState] = useState();

  // Effects
  useEffect(() => {
    // ...
  }, []);

  // Handlers
  const handleClick = () => {
    // ...
  };

  // Render
  return (
    <div>
      {/* JSX */}
    </div>
  );
}
```

---

## 🚀 DEPLOYMENT

### **Build untuk Production:**
```bash
npm run build
```

Output di `dist/` folder

### **Deploy ke:**
- **Vercel**: Connected ke Git, auto-deploy
- **Netlify**: Drag-drop `dist/` folder
- **Traditional Hosting**: Upload `dist/` ke public_html
- **Docker**: Create Dockerfile dengan Node.js image

---

## 🐛 DEBUGGING TIPS

### **Use Console:**
```javascript
console.log('Data:', data);
console.error('Error:', error);
console.table(array);  // Lihat array dalam tabel
```

### **React DevTools:**
- Install extension di Chrome
- Inspect component state & props
- Time-travel debugging dengan record

### **Network Tab (F12):**
- Check API calls sebelum integrate backend
- Monitor request/response

---

## 📚 RESOURCES

- **React Docs**: https://react.dev
- **Tailwind CSS**: https://tailwindcss.com
- **Vite**: https://vitejs.dev
- **jsPDF**: https://github.com/parallax/jsPDF
- **html2canvas**: https://html2canvas.hertzen.com/

---

**Happy Coding!** 🎬💻✨

