# SI-Voting

SI-Voting adalah aplikasi web untuk mengelola proses voting secara digital, misalnya untuk pemilihan ketua, lomba, atau penilaian acara di lingkungan kampus maupun organisasi.

## Daftar Isi
- [Fitur Utama](#fitur-utama)
- [Arsitektur Aplikasi](#arsitektur-aplikasi)
- [Prasyarat](#prasyarat)
- [Cara Menjalankan Aplikasi](#cara-menjalankan-aplikasi)
  - [Menjalankan dengan Docker Compose](#menjalankan-dengan-docker-compose)
  - [Menjalankan Secara Manual](#menjalankan-secara-manual)
- [Struktur Proyek](#struktur-proyek)
- [Teknologi yang Digunakan](#teknologi-yang-digunakan)
- [Penggunaan Singkat](#penggunaan-singkat)
- [Catatan Pengembangan](#catatan-pengembangan)

## Fitur Utama
- Pembuatan dan manajemen event voting (judul, deskripsi, jadwal, dll).
- Registrasi dan autentikasi pengguna (panitia dan peserta).
- Proses voting online yang aman per akun.
- Dashboard real-time untuk memonitor jumlah suara dan statistik.
- Manajemen kandidat / opsi voting per event.
- Riwayat event yang pernah diikuti/dibuat.

## Arsitektur Aplikasi
Proyek ini terdiri dari beberapa komponen:

- `clientapp`: aplikasi web client berbasis Spring Boot dengan template HTML, CSS, dan JavaScript.
- `serverapp`: layanan backend Spring Boot yang menangani logika bisnis, penyimpanan data, dan proses voting.
- `deploy`: konfigurasi pendukung deployment (misalnya `nginx-client-proxy.conf`).
- `materials`: materi tambahan seperti skema database (`database.sql`).

Komponen-komponen tersebut dapat dijalankan secara terpisah atau melalui `docker-compose.yml` untuk lingkungan tercontainerisasi.

## Prasyarat
Sebelum menjalankan aplikasi, pastikan:

- Java 17 (atau versi yang sesuai dengan konfigurasi di `pom.xml`).
- Maven atau wrapper bawaan (`mvnw` / `mvnw.cmd`).
- Docker & Docker Compose (opsional, jika ingin menjalankan dengan container).
- Git (opsional, untuk clone repository).

## Cara Menjalankan Aplikasi

### Menjalankan dengan Docker Compose
1. Pastikan Docker dan Docker Compose sudah terpasang dan berjalan.
2. Dari root proyek (`Si-Voting`), jalankan:

```powershell
cd c:\Users\LENOVO\Documents\GitHub\Si-Voting
docker-compose up --build
```

3. Aplikasi client dan server akan dibuild dan dijalankan sesuai konfigurasi di `docker-compose.yml`.
4. Buka browser dan akses URL yang tertera di log (misalnya `http://localhost:8080` atau sesuai konfigurasi).

### Menjalankan Secara Manual

#### 1. Menjalankan `serverapp`
```powershell
cd c:\Users\LENOVO\Documents\GitHub\Si-Voting\serverapp
./mvnw spring-boot:run
```
(atau `mvnw.cmd spring-boot:run` di Windows Command Prompt.)

#### 2. Menjalankan `clientapp`
```powershell
cd c:\Users\LENOVO\Documents\GitHub\Si-Voting\clientapp
./mvnw spring-boot:run
```

Setelah kedua layanan berjalan, akses aplikasi melalui URL client (misalnya `http://localhost:8080` atau port yang dikonfigurasi di `application.yaml`).

## Struktur Proyek

Secara garis besar, struktur direktori proyek:

```text
Si-Voting/
├─ docker-compose.yml
├─ start.sh
├─ clientapp/
│  ├─ Dockerfile
│  ├─ pom.xml
│  ├─ src/main/java/com/example/clientapp/...
│  ├─ src/main/resources/
│  │  ├─ application.yaml
│  │  ├─ static/ (CSS, JS, assets)
│  │  └─ templates/ (HTML: login, register, dashboard, dll.)
├─ serverapp/
│  ├─ Dockerfile
│  ├─ pom.xml
│  ├─ src/main/java/com/example/... (logika backend)
│  ├─ src/main/resources/application.properties
├─ deploy/
│  └─ nginx-client-proxy.conf
├─ materials/
│  └─ database.sql
└─ uploads/
```

## Teknologi yang Digunakan
- **Backend**: Spring Boot (Java)
- **Frontend**: Spring Boot MVC + Thymeleaf / template engine HTML, CSS (custom), JavaScript (modular: `api.js`, `utils.js`, `websocket.js`).
- **Database**: Sesuai konfigurasi di `application.properties` / `application.yaml` (lihat file tersebut untuk detail).
- **Containerization**: Docker, Docker Compose.
- **Web Server / Proxy**: Nginx (melalui konfigurasi di folder `deploy`).

## Penggunaan Singkat
1. **Login / Registrasi**
   - Akses halaman login (`/login`) untuk masuk.
   - Pengguna baru dapat mendaftar melalui halaman register (`/register`) jika fitur ini diaktifkan.

2. **Dashboard & Event**
   - Setelah login, pengguna diarahkan ke dashboard utama.
   - Panitia dapat membuat event baru (judul, deskripsi, jadwal, opsi voting) melalui menu manajemen event.
   - Peserta dapat melihat daftar event yang tersedia dan masuk ke halaman detail event untuk melakukan voting.

3. **Voting & Hasil**
   - Voting dilakukan langsung dari halaman event.
   - Hasil voting ditampilkan secara real-time pada dashboard (jumlah suara per opsi dan status event).

## Catatan Pengembangan
- Untuk mengubah konfigurasi port, database, atau properti lainnya, lihat:
  - `clientapp/src/main/resources/application.yaml`
  - `serverapp/src/main/resources/application.properties`
- Untuk menambahkan atau mengubah tampilan halaman, ubah file di:
  - `clientapp/src/main/resources/templates/`
  - `clientapp/src/main/resources/static/css/` dan `static/js/`.
- Ubah struktur tabel dan relasi database melalui `materials/database.sql` dan entity di paket Java terkait.

---

Jika Anda membutuhkan dokumentasi tambahan (misalnya ERD database, diagram arsitektur, atau panduan deployment produksi), dapat ditambahkan pada bagian baru di README ini atau di folder `materials/`.