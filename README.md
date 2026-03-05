# React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react) uses [Babel](https://babeljs.io/) (or [oxc](https://oxc.rs) when used in [rolldown-vite](https://vite.dev/guide/rolldown)) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh

## React Compiler

The React Compiler is not enabled on this template because of its impact on dev & build performances. To add it, see [this documentation](https://react.dev/learn/react-compiler/installation).

## App Features

This simple cinema ticketing application was enhanced with the following capabilities:

- 🎬 **CRUD film**: tambah, edit, hapus data film langsung dari beranda dengan tombol "Kelola Film". Poster film kini dapat diunggah dari berkas lokal (upload gambar), bukan lagi melalui URL.
- 🪑 **Pilih kursi**: layout kursi interaktif, maksimum 5 per transaksi, kursi yang sudah dibooking tidak bisa dipilih lagi.
- ⏰ **Jam tayang**: pilih jam ketika pembelian tiket.
- 🍿 **Pembelian snack/minuman**: pilih popcorn, soda, nachos, dan tentukan jumlah.
- 💳 **Checkout & pembayaran**: ringkasan harga otomatis ditampilkan dan transaksi disimulasikan.
- 📄 **Cetak nota/tiket PDF**: setiap tiket bisa didownload sebagai file PDF, sekarang mencantumkan total pembayaran dan detail lengkap.
- 📌 **Detail tiket lebih kaya**: klik "Baca selengkapnya" di halaman Tiket untuk melihat judul, jam, kursi, snack, dan total biaya.
- 👤 **Halaman profil penuh**: edit profil termasuk upload foto, notifikasi, pengaturan (tema gelap & notifikasi), bantuan & FAQ, tombol keluar, serta mekanisme login dan logout sederhana.
- 🔐 **Login sederhana**: pengguna harus masuk dengan nama (opsional peran) untuk mengakses aplikasi; informasi disimpan di localStorage.
- 🎨 **Tema & notifikasi**: pengguna dapat mengaktifkan tema gelap dan mengelola notifikasi pada halaman pengaturan.
- 💾 **Auto-save / persistence**: semua data (user, movies, tickets, settings, booked seats) tersimpan di localStorage dan terpulihkan setelah refresh.

Untuk menjalankan proyek:

```bash
npm install
npm run dev
```

Silakan buka http://localhost:5173/ setelah server berjalan.

## Expanding the ESLint configuration

If you are developing a production application, we recommend using TypeScript with type-aware lint rules enabled. Check out the [TS template](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react-ts) for information on how to integrate TypeScript and [`typescript-eslint`](https://typescript-eslint.io) in your project.
