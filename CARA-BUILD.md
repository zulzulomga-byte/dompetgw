# Saku — Build ke APK

Project ini pakai Capacitor: semua isi app (HTML/CSS/JS + data localStorage)
dibungkus jadi WebView native. Hasilnya APK berdiri sendiri, jalan offline,
gak butuh hosting/internet sama sekali.

## Cara TERGAMPANG: build otomatis via GitHub Actions (gak install apa-apa)

1. Bikin repo baru di github.com (public atau private, bebas), misal `saku-app`.
2. Upload semua isi folder project ini ke repo itu — cara paling gampang:
   di halaman repo kosong, klik "uploading an existing file", drag semua
   file & folder dari hasil extract zip ini (termasuk folder `.github`).
   *(Folder `node_modules` gak usah diupload kalau ada — biarin aja gak
   ke-include, otomatis di-install ulang di cloud.)*
3. Setelah keupload / commit, buka tab **Actions** di repo tsb.
   Kalau belum otomatis jalan, klik workflow "Build APK" → **Run workflow**.
4. Tunggu ~3-5 menit sampai centang hijau.
5. Klik run yang selesai itu → scroll ke bawah ke bagian **Artifacts** →
   download **saku-apk.zip** → extract → dapet `app-debug.apk`.
6. Pindahin apk itu ke HP (Google Drive / kabel USB / dsb), buka, install.
   Kalau muncul warning "unknown source", izinin aja di setting HP.

Ulangi dari langkah 2-5 tiap kali kamu ubah isi `www/index.html` dan mau
apk baru (commit ulang → Actions jalan otomatis lagi).

---

## Alternatif: build manual pake Android Studio di laptop

## Yang perlu disiapkan (sekali aja)
1. Install **Node.js** (https://nodejs.org) — buat jalanin `npm install`.
2. Install **Android Studio** (gratis): https://developer.android.com/studio
3. Pas pertama buka, biarin dia download Android SDK bawaan (proses otomatis).

## Cara build APK
1. Extract zip project ini.
2. Buka terminal di folder project (root, yang ada `package.json`), jalanin:
   ```
   npm install
   ```
   (ini wajib — project Android-nya nge-refer ke folder `node_modules` buat
   library Capacitor, jadi jangan di-skip.)
3. Buka folder **android/** (bukan folder root) pakai Android Studio
   → File → Open → pilih folder `android`.
4. Tunggu Gradle sync selesai (bar loading di bawah, bisa beberapa menit di awal).
5. Di menu atas: **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
6. Setelah selesai, klik notifikasi "locate" atau cari manual di:
   `android/app/build/outputs/apk/debug/app-debug.apk`
7. Copy file `app-debug.apk` itu ke HP (lewat kabel USB, Google Drive, dsb),
   buka di HP → Install. (Kalau muncul warning "unknown source", izinin aja
   di setting HP — normal buat apk yang belum dari Play Store.)

## Kalau mau ganti isi tampilan / fitur nanti
Edit file di folder `www/index.html`, lalu jalanin ini dari root project
(butuh Node.js terinstall):
```
npx cap sync android
```
Baru build ulang APK-nya dari Android Studio kayak langkah di atas.

## Ganti icon app
Icon ada di `android/app/src/main/res/mipmap-*/ic_launcher*.png`.
Ganti isinya kalau mau desain icon lain, ukurannya udah harus persis sama
per folder (mdpi 48px, hdpi 72px, xhdpi 96px, xxhdpi 144px, xxxhdpi 192px).

## Catatan soal data
Data tersimpan di localStorage WebView Android — aman selama app gak
di-uninstall / data app-nya gak dihapus manual dari Settings HP.
Belum ada fitur backup/export, jadi hati-hati kalau nanti mau ganti HP.
