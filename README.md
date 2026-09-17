## Skenario Proyek PBL: SSH Brute Force Attack

Pada proyek ini, kelompok kami memilih skenario serangan **SSH Brute Force**. Skenario ini bertujuan untuk mendemonstrasikan bagaimana celah keamanan pada layanan *remote login* dapat dieksploitasi, serta bagaimana sistem pemantauan dapat mendeteksi anomali serangan tersebut secara *real-time*.

**Detail Skenario:**
* **Target Node:** Ubuntu Server / CyberOps Workstation dengan layanan SSH (Port 22) yang terbuka tanpa proteksi *Intrusion Prevention* (seperti Fail2ban).
* **Attacker Node:** Kali Linux. *Red Team* akan menggunakan *tools* otomatis (seperti Hydra atau Ncrack) untuk melakukan serangan tebak kata sandi (*brute force credential*) secara masif.
* **Monitoring Node:** Security Onion. *Blue Team* akan memantau lonjakan paket SYN di jaringan serta menganalisis indikasi kompromi (IoC) dari tumpukan *log* `Failed Password` pada server target.

Skenario ini dipilih karena memberikan visualisasi *traffic* serangan yang jelas untuk proses analisis pertahanan jaringan.
