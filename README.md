# Responsive Dashboard

<p align="center">
  <img src="https://img.shields.io/badge/Status-Active-brightgreen" />
  <img src="https://img.shields.io/badge/React%20Native-0.82-61DAFB" />
  <img src="https://img.shields.io/badge/Expo-~51-000020" />
  <img src="https://img.shields.io/badge/TypeScript-5.x-3178C6" />
  <img src="https://img.shields.io/badge/License-MIT-orange" />
</p>

**Responsive Dashboard** adalah aplikasi React Native (Expo + TypeScript) yang mendukung tampilan Mobile, Tablet, dan Large Tablet dengan konsep responsive dan orientation-aware layout. Project ini juga mengintegrasikan fitur *Todo* berbasis SQLite (expo-sqlite) sebagai bagian dari tugas Pemrograman Mobile.

---

## Fitur Utama

* Responsive layout untuk Mobile dan Tablet
* Breakpoint khusus untuk Large Tablet (≥ 1024px)
* Orientation-aware layout (Portrait dan Landscape)
* Grid card responsif (1 kolom → 2 kolom pada lebar tertentu)
* Icon responsif pada setiap card (@expo/vector-icons)
* SafeAreaView support untuk area notch/status bar
* **Todo local storage** menggunakan `expo-sqlite` dengan fitur:

  * Kolom `finished_at` (waktu selesai)
  * Migration safe (ALTER TABLE jika kolom belum ada)
  * Filter by status: All / Done / Undone
  * Checkbox untuk toggle status
  * Fungsi tambahan: search, date-range query, debug print

---

## Preview Tampilan

Lihat folder `assets/screenshots` atau preview di README repo (jika tersedia). Screenshot untuk beberapa ukuran:

* Mobile Portrait
* Mobile Landscape
* Tablet Portrait
* Large Tablet

> Catatan: pada README repo ini gambar ditampilkan dengan raw GitHub link agar muncul langsung.

---

## Teknologi yang Digunakan

* React Native
* Expo
* TypeScript
* `expo-sqlite` (local DB)
* `react-native-safe-area-context`
* `@expo/vector-icons` (Feather)

---

## Cara Menjalankan Project

1. Clone repository

```bash
git clone https://github.com/username/rn-responsive-dashboard.git
cd rn-responsive-dashboard
```

2. Install dependency

```bash
npm install
```

3. Jalankan project

```bash
npx expo start
```

4. Pilih metode menjalankan aplikasi:

* Tekan `a` untuk Android Emulator
* Tekan `w` untuk Web Browser
* Scan QR Code untuk perangkat fisik

---

## Struktur & Breakpoint

| Mode         | Lebar Layar |
| ------------ | ----------: |
| Mobile       |     < 768px |
| Tablet       |     ≥ 768px |
| Large Tablet |    ≥ 1024px |

Perilaku orientation-aware:

* Mobile Portrait: 1 kolom
* Mobile Landscape: 2 kolom
* Tablet: 2 kolom pada semua orientasi

---

## Todo (SQLite) — Ringkasan Integrasi

`app/services/todoService.ts` menyertakan API berikut (sudah diimplementasikan dalam tugas):

* `initDB()` — buat tabel `todos` dan migration safe untuk `finished_at` + index pada `finished_at`
* `getTodos(status?)` — ambil daftar (status: `all` | `done` | `undone`)
* `getTodoById(id)` — ambil satu todo
* `addTodo(text, done?)` — tambah todo; jika `done=1` maka `finished_at` otomatis diisi
* `updateTodo(id, fields)` — update teks atau `done`; otomatis set/clear `finished_at`
* `deleteTodo(id)` — hapus
* `getTodosByDateRange(start, end, opts?)` — ambil berdasarkan rentang `finished_at` atau `created_at`
* `searchTodos(q)` — cari teks
* `debugPrintTodos()` — debug helper

**Catatan migrasi:** `initDB()` menjalankan `PRAGMA table_info(todos)` dan `ALTER TABLE` jika `finished_at` belum ada.

---

## Komponen Utama

* `TodoList` (screen)

  * Menampilkan list, checkbox, tombol Edit / Delete
  * Filter UI (All / Done / Undone) berada di bawah input
  * Menampilkan `finished_at` jika ada
* `Checkbox` — custom tanpa library tambahan (TouchableOpacity + View)

Kode komponen dan service sudah disesuaikan agar kompatibel dengan TypeScript dan typing `expo-sqlite` (`getAllAsync`, `runAsync`).

---

## Contoh Quick Usage (Todo)

```ts
await initDB();
await addTodo("Beli kopi");
await addTodo("Selesai tugas", 1); // langsung done
const done = await getTodos("done");
await updateTodo(3, { done: 1 }); // sets finished_at
await updateTodo(3, { done: 0 }); // clears finished_at
```

---

## Skrip Berguna

* `npm start` — menjalankan expo
* `npm run android` / `npm run ios` — bila dikonfigurasi

---

## Kontribusi

Silakan ajukan PR atau isu. Untuk tugas kelas, tambahkan keterangan perubahan agar penguji mudah menilai.

---

## Author

Ranggis

---

## Lisensi

MIT
