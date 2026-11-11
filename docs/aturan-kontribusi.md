# Aturan kontribusi bersama dan konfigurasi pribadi

Aturan ini berlaku untuk semua developer dan agent. Instruksi pribadi tidak boleh menambah kewajiban akun, publikasi, atau layanan eksternal bagi kontributor lain. Cloud Sync produk berbeda dari pencatatan portfolio developer; kebijakan ini tidak mengubah fitur sinkronisasi aplikasi.

## Berkas yang disimpan di Git

| Materi | Aturan |
| --- | --- |
| Kode, tes, skrip build, manifest dan lockfile | Commit perubahan relevan sesuai package manager proyek; desktop memakai Yarn. Jangan memperbarui lockfile lain tanpa kebutuhan. |
| `AGENTS.md`, `DESIGN.md`, panduan arsitektur dan skill proyek | Commit bila berlaku bagi semua kontributor, tidak memuat rahasia, dan dapat diakses dari checkout. |
| `CHANGELOG.md` dan `releases/` | Commit catatan perubahan produk. JSON rilis mengikuti struktur yang sudah dikonsumsi aplikasi; catatan teknis tetap boleh ada di changelog. |
| Contoh konfigurasi | Commit contoh berisi placeholder, tanpa token atau identitas akun nyata. |
| `.gitmodules` | Hanya untuk dependency bersama yang diperlukan dan disepakati. Setiap gitlink wajib punya path, URL yang bisa diakses tim, serta commit yang tersedia. |

## Berkas yang tetap lokal

| Materi | Tempat dan aturan |
| --- | --- |
| Panduan pribadi | `AGENTS.local.md` atau `.agents/local/`; diabaikan Git. Agent membacanya hanya saat diminta pemilik, bukan sebagai kewajiban bersama. |
| Skill portfolio `activity-sync` | Di luar repo (diutamakan) atau `.agents/skills/activity-sync/` yang diabaikan; bukan submodule bersama. |
| Token portfolio dan konfigurasi akun | Secret store atau environment lokal; `.ACTIVITY_PROJECT_KEYS` dan `.env.*` diabaikan, kecuali berkas contoh tanpa rahasia. |
| Catatan kerja pribadi, path mesin, log publikasi | `.agents/local/` atau di luar repo. Jangan menyisipkan path mesin sendiri sebagai prasyarat bersama. |
| Dependency dan hasil build desktop | `node_modules/`, `.vite/`, `dist/`, `out/`, log dan hasil tes tetap mengikuti `.gitignore`. |

Mengabaikan berkas tidak menghapus berkas yang sudah terlacak atau rahasia dari histori. Tinjau diff; bila kredensial pernah bocor, pemilik perlu mencabut atau merotasinya melalui prosedur layanan terkait. Jangan menyalin kredensial ke contoh, output, atau catatan rilis.

## Tanggung jawab aksi

- Semua kontributor: ikuti arsitektur dan desain sesuai lingkup, lindungi data, jalankan pemeriksaan relevan, serta sertakan bukti dan batas verifikasi. Tidak wajib memiliki akun portfolio developer lain.
- Penulis perubahan: perbarui changelog dan rilis bila perilaku produk berubah. Perubahan panduan internal tidak memerlukan rilis aplikasi atau pengiriman aktivitas.
- Maintainer yang berwenang: meninjau dan menggabungkan kontribusi, memperbarui pointer submodule, serta menjalankan rilis/deploy sesuai otorisasi proyek. Kontribusi tidak otomatis memberi izin push atau deploy.
- Pemilik integrasi pribadi: memasang skill dan kredensialnya sendiri, memilih materi publik, serta menjalankan dan memverifikasi pengiriman ke akunnya. Delegasi harus eksplisit dan menyebut tujuan serta materi. Kegagalan layanan pribadi tidak menggagalkan tes, build, atau kontribusi tim.
- CI: memakai konfigurasi bersama dan secret yang sengaja disediakan untuk CI; tidak memuat instruksi atau mengirim aktivitas portfolio pribadi.

## Checkout dan submodule

Dari repo induk, jalankan:

```sh
git submodule update --init --recursive
```

Skill pribadi tidak boleh muncul sebagai gitlink. Jika perlu dilepas, pastikan tidak ada perubahan atau rahasia lokal yang hilang, keluarkan hanya entri tersebut dari indeks, dan tambahkan aturan ignore. Menghapus gitlink tidak memerlukan penghapusan isi lokal.

Perubahan `app-desktop` harus dikomit di repositori desktop lebih dahulu. Setelah commit tersedia di remote yang dapat diakses tim, maintainer mengomit pointer baru di repo induk. Jangan mengomit pointer ke commit lokal yang belum tersedia. `.gitignore` dalam repo induk tidak menggantikan `.gitignore` desktop.

## Pemeriksaan sebelum commit

Tinjau `git status`, `git diff`, dan `git diff --cached` pada kedua repo. Pilih hanya berkas dalam lingkup pekerjaan. Jalankan `git diff --check`; pastikan berkas pribadi terabaikan dan tidak ada gitlink tanpa deklarasi `.gitmodules`. Untuk kode desktop jalankan pemeriksaan proyek yang relevan dengan Yarn; bila Yarn tidak ada di PATH tetapi Corepack tersedia, gunakan `corepack yarn`. Jangan menandai pemeriksaan berhasil sebelum menjalankannya.
