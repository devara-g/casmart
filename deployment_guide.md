# 🚀 Panduan Deploy Casmart — Full Gratis

Semua layanan yang digunakan di panduan ini **100% GRATIS** (tier gratis).

**Stack Deploy:**
- 🗄️ **Database (MySQL)** → Railway
- ⚙️ **Backend (Node.js)** → Railway
- 🌐 **Frontend (React/Vite)** → Vercel

---

## Tahap 1: Persiapan — Export Database Lokal

Sebelum deploy, kamu harus export databasemu dulu dari HeidiSQL.

1. Buka **HeidiSQL**, pilih koneksi `casmart_db` kamu.
2. Klik kanan database `casmart_db` → **Export database as SQL**.
3. Di bagian **Output**, pilih `File` dan simpan sebagai `casmart.sql`.
4. Pastikan centang **Structure** dan **Data** (biar tablenya + datanya ikut).
5. Klik **Export**. Simpan file `.sql` tersebut.

---

## Tahap 2: Deploy Backend + Database ke Railway

**Railway** memungkinkan kamu menjalankan Backend Node.js dan Database MySQL sekaligus secara gratis.

### 2a. Buat Akun & Project Railway
1. Buka [railway.app](https://railway.app) → Login dengan **GitHub**.
2. Klik **New Project**.

### 2b. Tambahkan Database MySQL
1. Di dalam project, klik **+ Add a service** → **Database** → **MySQL**.
2. Tunggu beberapa detik sampai database siap (statusnya hijau).
3. Klik service MySQL yang baru dibuat → tab **Variables**. Catat nilai-nilai berikut:
   - `MYSQLDATABASE`
   - `MYSQLHOST`
   - `MYSQLPASSWORD`
   - `MYSQLPORT`
   - `MYSQLUSER`

### 2c. Import Tabel ke Database Railway
1. Buka **HeidiSQL** di laptopmu.
2. Buat koneksi baru dengan mengisi data dari variabel Railway di atas:
   - **Hostname**: isi `MYSQLHOST`
   - **Port**: isi `MYSQLPORT`
   - **User**: isi `MYSQLUSER`
   - **Password**: isi `MYSQLPASSWORD`
   - **Database**: isi `MYSQLDATABASE`
3. Setelah terhubung, klik menu **File** → **Run SQL file** → pilih file `casmart.sql` yang tadi sudah di-export.
4. Semua tabel akan otomatis dibuat di database Railway. ✅

### 2d. Upload Backend ke GitHub
1. Buat repository **baru** di GitHub (misal: `casmart-backend`).
2. Di dalam folder `backend` proyekmu, buka terminal dan jalankan:
   ```bash
   git init
   git add .
   git commit -m "first commit"
   git branch -M main
   git remote add origin https://github.com/USERNAME/casmart-backend.git
   git push -u origin main
   ```

### 2e. Deploy Backend ke Railway
1. Kembali ke project di Railway → **+ Add a service** → **GitHub Repo**.
2. Pilih repository `casmart-backend` yang baru saja kamu buat.
3. Railway akan otomatis mendeteksi ini adalah project Node.js.
4. Klik service backend kamu → tab **Variables** → **+ Add variable**. Tambahkan semua ini:

   | Key | Value |
   |---|---|
   | `DB_HOST` | Salin dari `MYSQLHOST` |
   | `DB_PORT` | Salin dari `MYSQLPORT` |
   | `DB_USER` | Salin dari `MYSQLUSER` |
   | `DB_PASSWORD` | Salin dari `MYSQLPASSWORD` |
   | `DB_NAME` | Salin dari `MYSQLDATABASE` |
   | `JWT_SECRET` | Teks acak panjang, contoh: `casmart-prod-secret-2025-!@#$` |
   | `GOOGLE_CLIENT_ID` | Client ID Google kamu |
   | `MIDTRANS_SERVER_KEY` | Server Key Midtrans kamu |
   | `MIDTRANS_CLIENT_KEY` | Client Key Midtrans kamu |
   | `GROQ_API_KEY` | API Key Groq kamu (untuk chatbot) |

5. Klik tab **Settings** → di bagian **Networking** → klik **Generate Domain**. Railway akan memberikan URL backend, contoh: `https://casmart-backend-production.up.railway.app`. **Simpan URL ini!** ✅

---

## Tahap 3: Deploy Frontend ke Vercel

### 3a. Upload Frontend ke GitHub
1. Buat repository **baru** di GitHub (misal: `casmart-frontend`).
2. Di dalam folder `frontend`, buka terminal dan jalankan:
   ```bash
   git init
   git add .
   git commit -m "first commit"
   git branch -M main
   git remote add origin https://github.com/USERNAME/casmart-frontend.git
   git push -u origin main
   ```

### 3b. Deploy ke Vercel
1. Buka [vercel.com](https://vercel.com) → Login dengan **GitHub**.
2. Klik **Add New Project** → Import repository `casmart-frontend`.
3. Vercel otomatis mendeteksi ini adalah project **Vite**.
4. Sebelum klik Deploy, buka bagian **Environment Variables** dan tambahkan:

   | Key | Value |
   |---|---|
   | `VITE_API_URL` | URL Railway backend kamu (dari langkah 2e) |
   | `VITE_GOOGLE_CLIENT_ID` | Client ID Google kamu |

5. Klik **Deploy**. Tunggu ~1-2 menit. Setelah selesai, Vercel memberimu link website, contoh: `https://casmart-frontend.vercel.app`. ✅

---

## Tahap 4: Konfigurasi Akhir (Wajib!)

Setelah semua berhasil di-deploy, lakukan 2 hal ini:

### Update Google Cloud Console
- Buka [console.cloud.google.com](https://console.cloud.google.com) → Credentials → OAuth Client ID kamu.
- Tambahkan URL Vercel kamu ke **Authorized JavaScript origins**: `https://casmart-frontend.vercel.app`
- Jika tidak dilakukan, Login dengan Google akan Error 401.

### Update Midtrans Notification URL
- Buka [dashboard.midtrans.com](https://dashboard.midtrans.com) → Settings → Configuration.
- Isi **Payment Notification URL** dengan:
  `https://casmart-backend-production.up.railway.app/api/payment-notification`

---

## Ringkasan Biaya

| Layanan | Plan | Biaya |
|---|---|---|
| Railway (DB + Backend) | Gratis $5 credit/bulan | Rp 0 |
| Vercel (Frontend) | Hobby (Gratis) | Rp 0 |
| **Total** | | **Rp 0** |

> Railway memberikan $5 credit gratis per bulan. Untuk project kecil seperti ini biasanya sudah lebih dari cukup.
