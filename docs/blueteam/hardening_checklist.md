# Blue Team Hardening Checklist (Target Node - Kelompok 14)

Daftar kontrol keamanan (*hardening*) minimum yang diterapkan pada Target Node (`192.168.17.5`). Screenshot bukti fisik disimpan di `docs/blueteam/evidence/`.

| Kategori | Kontrol Hardening | Target Service | Status | Bukti File di GitHub |
| :--- | :--- | :--- | :--- | :--- |
| **A. Access & Auth** | Nonaktifkan login root langsung | OpenSSH Server | **Sudah** | [ssh_key_auth.png](evidence/ssh_key_auth.png) |
| **A. Access & Auth** | SSH key-based auth  | OpenSSH Server | **Sudah** | [fail2ban_status.png](evidence/fail2ban_status.png) |
| **A. Access & Auth** | Rate-limit percobaan login | OpenSSH Server | **Sudah** | [fail2ban_status.png](evidence/fail2ban_status.png) |
| **B. Service & Surface** | Nonaktifkan service/port yang tidak dipakai | UFW Firewall | **Sudah** | [ufw_status.png](evidence/ufw_status.png) |
| **B. Service & Surface** | firewall (ufw/iptables) whitelist port perlu saja | Nginx Web Server | **Sudah** | [nginx_tokens.png](evidence/nginx_tokens.png) |
| **B. Service & Surface** |  update & patch OS/paket ke versi aman | Nginx Web Server | **Sudah** | [rate_limit.png](evidence/rate_limit.png) |
| **C. Data & App** | Isolasi Total Port 3306 MySQL (Docker `appnet` Internal Only, No Published Port) | MySQL Docker | **Belum** | [docker_compose.png](evidence/docker_compose.png) |
| **C. Data & App** | Enkripsi kredensial DB via file `.env` & set izin akses ketat (`chmod 600 .env`) | Backend / Environment | **Belum** | [env_chmod.png](evidence/env_chmod.png) |
