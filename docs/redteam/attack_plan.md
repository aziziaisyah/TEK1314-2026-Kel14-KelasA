# Attack Plan Kelompok 14

| Jenis Serangan | Target | Metode | Tools | Tujuan |
|---|---|---|---|---|
| Web Application Attack | Dasbor Web IoT | SQL Injection | SQLMap / Manual Payload | Menembus halaman login untuk memanipulasi kontrol perangkat IoT dari antarmuka web |
| Authentication Attack | Layanan OpenSSH | SSH Brute Force & Credential Stuffing | THC-Hydra / Ncrack + rockyou.txt | Menebak sandi user server secara masif untuk mengambil alih kendali sistem operasi sepenuhnya |
| Availability Attack | Infrastruktur Jaringan (Target Node) | TCP SYN Flood | Hping3 / Metasploit | Membanjiri server dengan paket palsu hingga resource habis, menyebabkan web IoT down dan hilang visibilitas |
