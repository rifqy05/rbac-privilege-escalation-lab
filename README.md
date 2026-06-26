# 🛡️ Role-Based Access Control (RBAC) Privilege Escalation & Forensic Investigation

## 📌 Executive Summary
Proyek ini adalah demonstrasi *penetration testing* dan audit keamanan pada sistem operasi Linux (Ubuntu Server) menggunakan pendekatan *Assumed Breach*. Tujuannya adalah untuk membuktikan bagaimana kredensial pengguna biasa dengan hak akses rendah (perawat) dapat digunakan untuk mengeksploitasi kerentanan konfigurasi sistem (*Sudo* dan *Cron Job*) hingga mengambil alih sistem secara penuh (*Root*), serta bagaimana melacak jejak serangan tersebut melalui analisis log.

> **Tonton Video Demonstrasi Serangannya di sini:**
> https://www.youtube.com/watch?v=GPc9Qhvii1M

## 🏗️ Topologi & Environment
Lab ini dibangun secara lokal dan terisolasi menggunakan VirtualBox (Host-Only Adapter).
* **Attacker:** Kali Linux
* **Target:** Ubuntu Server 22.04 LTS (Tanpa GUI)
* **Sistem Logging:** `auditd` untuk pemantauan eskalasi hak istimewa.

## ⚔️ Attack Chain (Rantai Serangan)
Serangan dilakukan melalui beberapa fase:
1. **Initial Access:** Validasi akses awal dilakukan melalui serangan *Brute-Force* SSH menggunakan `hydra` terhadap kredensial yang lemah.
2. **Foothold:** Berhasil masuk ke dalam sistem menggunakan akun `nurse_bob`.
3. **Privilege Escalation 1 (Sudo Misconfiguration):** Mengeksploitasi hak akses *Sudo* tanpa *password* pada *binary* `/usr/bin/find` untuk membaca data rekam medis pasien yang bersifat rahasia.
4. **Privilege Escalation 2 (Root Access via Cron):** Membajak *script maintenance* otomatis (`/opt/scripts/maintenance.sh`) yang memiliki *permission* `chmod 777`. Penyerang menyuntikkan *payload reverse shell* sehingga mendapatkan akses *root* penuh ke mesin penyerang.

## 🔎 Analisis Forensik
*(Tambahkan screenshot log dari server target di sini untuk membuktikan jejak IP penyerang atau manipulasi file)*

## 🛡️ Remediasi & Mitigasi
Untuk mencegah serangan serupa pada lingkungan produksi, rekomendasi keamanan berikut harus diterapkan:
1. **Terapkan Prinsip Least Privilege:** Hapus konfigurasi `NOPASSWD` pada *file* Sudoers. Pengguna biasa tidak boleh mengeksekusi *binary* yang berpotensi memunculkan *shell* (seperti `find`, `awk`, `less`) sebagai admin.
2. **Perketat File Permission:** Jangan pernah menggunakan `chmod 777`, terutama pada *script* atau *binary* yang dieksekusi otomatis oleh sistem (*Cron*) dengan hak akses *root*. Gunakan `chmod 750` atau `755` dengan kepemilikan yang tepat.
3. **Enforce Password Policy:** Terapkan kebijakan kata sandi yang kompleks untuk mencegah serangan *brute-force* pada tahap *Initial Access*.

---
*Proyek ini disusun oleh **Ahmad Rifqy Ramli** (Informatika - UPNVJ) sebagai bagian dari Ujian Akhir Semester Keamanan Data dan Jaringan.*
