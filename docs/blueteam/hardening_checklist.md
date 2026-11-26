# Blue Team Hardening Checklist (Target Node - Kelompok 14)

Daftar kontrol keamanan (*hardening*) minimum yang diterapkan. Screenshot bukti fisik disimpan di `docs/blueteam/evidence/`.

| Kategori | Kontrol Hardening  | Status | Bukti File di GitHub |
| :--- | :--- | :--- | :--- |
| **A. Access & Auth** | Nonaktifkan login root langsung | **Sudah** | [After_nonaktifkan_login_root.jpg](evidence/After_nonaktifkan_login_root.jpg) |
| **A. Access & Auth** | SSH key-based auth | **Sudah** | [After_SSH_key-based_auth.jpg](evidence/After_SSH_key-based_auth.jpg) |
| **A. Access & Auth** | Rate-limit percobaan login | **Sudah** | [After_verifikasi_rate-limit_dan_SSH_key-based_auth.jpg](evidence/After_verifikasi_rate-limit_dan_SSH_key-based_auth.jpg) |
| **B. Service & Surface** | Nonaktifkan service/port yang tidak dipakai | **Sudah** | [After_nonaktifkan_service_dan_port_yang_tidak_dipakai.jpeg](evidence/After_nonaktifkan_service_dan_port_yang_tidak_dipakai.jpeg) |
| **B. Service & Surface** | Firewall (ufw/iptables) whitelist port perlu saja | **Sudah** | [After_firewall_ufw_whitelist_port_22_dan_80.jpg](evidence/After_firewall_ufw_whitelist_port_22_dan_80.jpg) |
| **B. Service & Surface** | Update & patch OS/paket ke versi aman | **Sudah** | - |
| **C. Data & App** | Least privilege pada user DB | **Belum** | - |
| **C. Data & App** | Enkripsi/kredensial tidak hardcoded | **Belum** | - |
