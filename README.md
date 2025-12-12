# React Native Todo App (SQLite)

<p align="center">
  <img src="https://img.shields.io/badge/Status-Active-brightgreen" />
  <img src="https://img.shields.io/badge/React%20Native-0.82-61DAFB" />
  <img src="https://img.shields.io/badge/Expo-~51-000020" />
  <img src="https://img.shields.io/badge/SQLite-expo--sqlite-blue" />
  <img src="https://img.shields.io/badge/TypeScript-5.x-3178C6" />
  <img src="https://img.shields.io/badge/License-MIT-orange" />
</p>

## Ringkasan

Aplikasi Todo berbasis React Native (Expo) yang menggunakan SQLite (`expo-sqlite`) untuk penyimpanan lokal. Aplikasi ini dirancang untuk tugas Pemrograman Mobile dan mengutamakan stabilitas, kemudahan migrasi skema, dan antarmuka sederhana namun fungsional.

---

## Fitur Utama

* Operasi CRUD pada entitas Todo (Create, Read, Update, Delete).
* Toggle status dengan checkbox (done / undone).
* Kolom `finished_at` yang otomatis diisi saat todo ditandai selesai.
* Filter daftar berdasarkan status: `all`, `done`, `undone`.
* Migrasi skema otomatis: menambahkan kolom `finished_at` jika belum ada.
* Fungsi utilitas: pencarian teks (`searchTodos`), query rentang tanggal (`getTodosByDateRange`), dan debug print (`debugPrintTodos`).
* UI menggunakan `FlatList`, input teks, tombol edit/hapus, dan komponen checkbox kustom.

---

## Preview Fitur Filter

Tabel berikut menampilkan tiga mode filter yang tersedia pada aplikasi Todo: All, Done, dan Undone.

<table align="center">
  <tr>
    <th>No</th>
    <th>Fitur</th>
    <th>Preview</th>
  </tr>
  <tr>
    <td>1</td>
    <td>All</td>
    <td><img src="https://github.com/Ranggis/Api-Image/raw/main/todo_all.png" width="260" /></td>
  </tr>
  <tr>
    <td>2</td>
    <td>Done</td>
    <td><img src="https://github.com/Ranggis/Api-Image/raw/main/todo_done.png" width="260" /></td>
  </tr>
  <tr>
    <td>3</td>
    <td>Undone</td>
    <td><img src="https://github.com/Ranggis/Api-Image/raw/main/todo_undone.png" width="260" /></td>
  </tr>
</table>

---

## Teknologi

* React Native
* Expo
* TypeScript
* `expo-sqlite`
* `react-native-safe-area-context`
* `@expo/vector-icons` (opsional)

---

## Struktur Proyek (ringkasan)

```
app/
├─ components/
│  └─ TodoList.tsx        # Komponen layar utama todo
├─ services/
│  └─ todoService.ts      # Semua fungsi database dan utilitas
├─ assets/
│  └─ images/
├─ app.json
├─ package.json
└─ tsconfig.json
```

---

## Skema Tabel (SQLite)

Tabel `todos` dibuat dengan kolom berikut:

```sql
CREATE TABLE IF NOT EXISTS todos (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  text TEXT NOT NULL,
  done INTEGER NOT NULL DEFAULT 0,
  created_at DATETIME DEFAULT (datetime('now')),
  finished_at DATETIME
);
```

Kolom `finished_at` bersifat nullable; jika `done = 1` maka `finished_at` diset menjadi `datetime('now')` — jika `done = 0` maka `finished_at` dikosongkan.

---

## API Service (ringkasan fungsi utama di `todoService.ts`)

* `initDB(): Promise<void>`

  * Membuat tabel `todos` bila belum ada.
  * Menjalankan migrasi ringan (menambahkan `finished_at` jika belum ada).
  * Membuat index `idx_todos_finished_at` untuk mempercepat query rentang tanggal.

* `getTodos(status?: 'all' | 'done' | 'undone'): Promise<Todo[]>`

  * Mengambil daftar todo sesuai status yang diminta.

* `getTodoById(id: number): Promise<Todo | null>`

  * Mengambil satu todo berdasarkan `id`.

* `addTodo(text: string, done?: number): Promise<number>`

  * Menambahkan todo baru; jika `done = 1` akan mengisi `finished_at`.

* `updateTodo(id: number, fields: { text?: string; done?: number }): Promise<void>`

  * Memperbarui teks atau status; secara otomatis set/clear `finished_at`.

* `deleteTodo(id: number): Promise<void>`

  * Menghapus todo berdasarkan `id`.

* `getTodosByDateRange(startISO, endISO, opts?): Promise<Todo[]>`

  * Mengambil todo berdasarkan rentang `finished_at` (atau `created_at` jika diatur).

* `searchTodos(q: string): Promise<Todo[]>`

  * Pencarian sederhana menggunakan `LIKE`.

* `debugPrintTodos(): Promise<void>`

  * Helper untuk menampilkan semua baris tabel pada console (untuk debug).

---

## Contoh Penggunaan (Quick Start)

1. Install dependency

```bash
npm install
```

2. Jalankan Metro / Expo

```bash
npx expo start
```

3. Inisialisasi DB (biasanya dipanggil saat app start)

```ts
import { initDB } from './app/services/todoService';
await initDB();
```

4. Operasi dasar

```ts
import { addTodo, getTodos, updateTodo, deleteTodo } from './app/services/todoService';

await addTodo('Beli kopi');
const all = await getTodos('all');
await updateTodo(1, { done: 1 });
await deleteTodo(2);
```

---

## Debugging & Verifikasi

* Gunakan `debugPrintTodos()` untuk melihat isi tabel pada console.
* Setelah toggle status, gunakan `getTodoById(id)` untuk memverifikasi `finished_at` terisi / bersih.
* Pastikan memanggil `initDB()` sekali di awal aplikasi (mis. di root `useEffect`).

---

## Praktik Terbaik & Catatan

* Pastikan `skipLibCheck: true` di `tsconfig.json` jika editor menunjukkan peringatan terkait `expo-module-scripts`.
* Untuk produksi, pertimbangkan enkripsi atau penyimpanan aman jika menyimpan data sensitif.
* Index pada `finished_at` membantu untuk query rentang tanggal besar.

---

## Kontribusi

Jika kamu ingin menambahkan fitur (mis. sinkronisasi ke remote, backup, atau fitur UI tambahan), ajukan PR dengan deskripsi perubahan dan alasan.

---

## Penulis

Ranggis
