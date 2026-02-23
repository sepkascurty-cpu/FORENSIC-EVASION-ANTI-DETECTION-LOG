# FORENSIC-EVASION-ANTI-DETECTION-LOG
> Focus: Log Cleaning, Process Masquerading, and Obfuscation
> Date: Feb 23, 2026
> Context: Post-Exploitation Research

---

## [ 01 ] LOG CLEANING TECHNIQUES
Setelah berhasil melakukan "Heist", langkah terpenting adalah menghapus jejak digital agar tidak terdeteksi oleh sistem SIEM atau Sysadmin.

* **Bash History Wipe**: Menghapus jejak perintah yang baru saja diketik.
  `export HISTSIZE=0 && history -c`
* **Log Scrubbing**: Menghapus baris spesifik yang mengandung IP kita di `/var/log/auth.log` menggunakan `sed`.
  `sed -i '/[YOUR_IP]/d' /var/log/auth.log`
* **Artifact Removal**: Menghapus tools yang di-download (seperti LinPeas atau reverse shells) secara permanen menggunakan `shred` agar tidak bisa di-recovery.

---

## [ 02 ] PROCESS MASQUERADING
Teknik menyembunyikan script `rat-sep.py` agar terlihat seperti proses sistem yang sah.

* **Process Renaming**: Mengubah nama proses di memori sehingga saat admin mengetik `ps aux`, script kita muncul sebagai `[kworker/u2:1]` atau `systemd-journal`.
* **Environment Hiding**: Menggunakan `unset HISTFILE` sebelum memulai sesi agar tidak ada file `.bash_history` yang tercipta.

---

## [ 03 ] MALWARE OBFUSCATION (PYTHON)
Eksperimen hari ini pada script `rat-sep.py` untuk menghindari deteksi Signature-Based AV:
1. **Base64 Encoding**: Mengencode payload sensitif agar tidak terbaca sebagai teks biasa oleh scanner.
2. **Variable Renaming**: Mengganti semua nama variabel menjadi string acak (misal: `bot_token` menjadi `a1_z9`).
3. **Dynamic Loading**: Memuat library penting hanya saat dibutuhkan (Runtime), bukan di awal script.



---

## [ 04 ] DEFENSIVE COUNTER-RESEARCH
Gimana cara deteksi teknik ini?
* **Immutable Logs**: Mengirim log ke server eksternal secara real-time sehingga penyerang tidak bisa menghapus log di mesin lokal.
* **Auditd**: Menggunakan daemon audit Linux untuk memantau setiap file yang dibuka atau dihapus oleh user dengan hak akses tinggi.

---
*Status: Stealth Mode Research*
*Author: sepkascurty-cpu*
