# Blue Team Hardening Checklist (Target Node - Kelompok 14)

Daftar kontrol keamanan (*hardening*) minimum yang diterapkan. Screenshot bukti fisik disimpan di `docs/blueteam/evidence/`.

| Kategori | Kontrol Hardening  | Status | Bukti File di GitHub |
| :--- | :--- | :--- | :--- |
| **A. Access & Auth** | Nonaktifkan login root langsung | **Sudah** | - |
| **A. Access & Auth** | SSH key-based auth | **Sudah** | - |
| **A. Access & Auth** | Rate-limit percobaan login | **Sudah** | - |
| **B. Service & Surface** | Nonaktifkan service/port yang tidak dipakai | **Sudah** | - |
| **B. Service & Surface** | Firewall (ufw/iptables) whitelist port perlu saja | **Sudah** | - |
| **B. Service & Surface** | Update & patch OS/paket ke versi aman | **Sudah** | - |
| **C. Data & App** | Least privilege pada user DB | **Belum** | - |
| **C. Data & App** | Enkripsi/kredensial tidak hardcoded | **Belum** | - |
