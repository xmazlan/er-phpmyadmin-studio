# Panduan & Konvensi AI Agent — xphpMyAdmin Desktop

> **PERINGATAN WAJIB DIPATUHI (MANDATORY POLICY)**:
> Seluruh AI Agent yang bekerja pada repositori `app-desktop` **WAJIB MEMATUHI DAN MENGEKSEKUSI** seluruh aturan arsitektur, standar estetika desain, prosedur pencatatan rilis, dan pelaporan kegiatan yang tertera pada berkas ini, [`DESIGN.md`](file:///DESIGN.md), serta skill bersama yang relevan di [`.agents/skills/`](.agents/skills/). Integrasi pribadi mengikuti [kebijakan kontribusi](docs/aturan-kontribusi.md).
> Pelanggaran terhadap standar desain, kelalaian pencatatan perubahan produk pada dokumen rilis bersama, maupun pelanggaran invarian teknis dilarang keras.

---

## 1. Kepatuhan Skill Wajib (.agents/skills/)

Gunakan panduan bersama sesuai lingkup perubahan. Perubahan dokumentasi saja tidak memerlukan pekerjaan UI atau akses layanan eksternal:

### 1. `raycast-design` ([`.agents/skills/raycast-design/SKILL.md`](file:///.agents/skills/raycast-design/SKILL.md) & [`DESIGN.md`](file:///DESIGN.md))
- **Kewajiban**: Standar estetika visual desktop developer tools dari [VoltAgent awesome-design-md (Raycast)](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/raycast).
- **Implementasi**: Canvas ultra-dark obsidian (`#07080a` / `#0a0d18`), permukaan bertingkat (`#0d0d0d` → `#101111` → `#121212`), hairline 1px borders (`#242728`), keycap `<kbd>` shortcuts, command-palette omni-search, dan tombol aksi utama kontras tinggi (`bg-white text-black`).
- **Aturan Eksekusi**: Setiap penambahan atau modifikasi komponen antarmuka harus mematuhi palet dan checklist pada skill ini.

### 2. `shadcn-ui` ([`.agents/skills/shadcn-ui/SKILL.md`](file:///.agents/skills/shadcn-ui/SKILL.md))
- **Kewajiban**: Standar arsitektur komponen UI, pustaka 62+ komponen di `@/components/ui/`, integrasi Tailwind CSS v4 + Radix UI, serta penggabungan class menggunakan utilitas `cn()` (`@/utils/tailwind`).
- **Aturan Eksekusi**: Dilarang membuat tombol atau modal unstyled mentah jika komponen resmi shadcn sudah tersedia di `src/components/ui/`.

### 3. Catatan rilis bersama dan integrasi pribadi
- Perubahan perilaku produk wajib dicatat di `CHANGELOG.md` dan, bila dipakai aplikasi, `releases/`. Ikuti struktur JSON yang sudah ada, pertahankan bahasa `id` dan `en`, dan jelaskan manfaat pengguna. Dokumentasi internal cukup dicatat di changelog bila material; jangan membuat rilis produk semu.
- Catatan rilis harus dapat dibuat tanpa skill pribadi, token, akun portfolio, atau layanan eksternal.
- `activity-sync` adalah integrasi portfolio pribadi opsional. Hanya pemilik akun atau developer yang mendapat delegasi eksplisit boleh menjalankannya; tidak menjadi tugas semua developer, CI, atau syarat menyelesaikan kontribusi.
- Tidak ada pengiriman otomatis berdasarkan `AGENTS.md` ini. Sebelum mengirim, pemilik menentukan tujuan, akun, dan materi publik yang disetujui. Jangan mengirim kode privat, kredensial, data pelanggan, atau mengatasnamakan kontributor lain.
- Simpan skill pribadi di luar repo atau direktori lokal yang diabaikan Git. Ketiadaannya tidak menghambat clone, pull, tes, build, dan pencatatan rilis bersama.
- Rincian berkas, tanggung jawab, dan alur submodule: [Aturan kontribusi](docs/aturan-kontribusi.md).

---

## 2. Invarian Arsitektur & Aturan Teknis Desktop

1. **Runtime & Modul Native**:
   - Runtime desktop adalah **Electron 44 / Node.js 22**.
   - Driver database menggunakan `mysql2/promise` murni (pure JavaScript) untuk menjamin kompatibilitas cross-platform tanpa memerlukan kompilasi native C++ (`node-gyp`).
   - SSH Tunneling menggunakan pustaka `ssh2` murni.
   - Manajemen paket menggunakan **Yarn** (`yarn.lock`).
2. **Kriptografi & Password Lifecycle**:
   - Master Key disimpan secara aman pada OS Keychain / DPAPI (`keytar` / `safeStorage`).
   - Kredensial database dienkripsi menggunakan AES-256-GCM.
   - Siklus password profil koneksi: opsi `keep` (pertahankan rahasia terenkripsi), `replace` (input password baru), dan `remove` (hapus password dari vault).
3. **Validasi & Integritas Data**:
   - Seluruh payload profil, konfigurasi SSH, dan pengaturan SSL/TLS wajib divalidasi menggunakan skema **Zod** (`src/types/connection.ts`) sebelum disimpan ke SQLite lokal.
4. **Isolasi Tab & Diagnostik Visual**:
   - Setiap tab koneksi database memiliki worker pool dan sesi tunnel terisolasi.
   - Diagnostik koneksi dijalankan dalam 4 tahap sekuensial yang terpantau secara visual:
     1. `socket`: TCP Socket Connection
     2. `handshake`: Protocol Handshake
     3. `auth`: Authentication & Permission Check
     4. `schema`: Metadata & Information Schema Discovery

---

## 3. Checklist Pre-Commit untuk AI Agent

Sebelum menyelesaikan tugas atau menjawab pengguna, AI Agent **WAJIB MEMASTIKAN**:
- [ ] Antarmuka mematuhi **Raycast Design System** (hairline borders 1px, continuous dark canvas, high-contrast primary CTA).
- [ ] Komponen UI memanfaatkan pustaka `@/components/ui/*` melalui `cn()` .
- [ ] Validasi data dilakukan dengan skema Zod.
- [ ] Tidak ada konflik atau warning dependency pada Yarn (`yarn.lock`).
- [ ] Untuk perubahan kode, jalankan pemeriksaan relevan dan unit test (`yarn test`); laporkan hasil sebenarnya. Untuk dokumentasi saja, periksa diff, tautan, dan aturan Git.
- [ ] Perbarui catatan rilis bersama bila perilaku produk berubah. Integrasi portfolio pribadi opsional dan tidak menghambat kontribusi.
