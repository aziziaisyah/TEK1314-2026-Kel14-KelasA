# Purple Team Log - Kelompok 14

## Skenario 1 dari 3 - Web Application Attack

- **Target:** Dasbor Web IoT (MUSH Field Console)
- **Metode:** SQL Injection, brute force kredensial, dan penulisan data sensor tanpa otorisasi
- **Tools:** Browser DevTools, curl, wordlist manual
- **Tujuan:** Menembus halaman login untuk memanipulasi kontrol perangkat IoT dari antarmuka web.

| Tahap | Red Team | Blue Team | Bukti |
| :--- | :--- | :--- | :--- |
| **Tahap 1 - Serang (Baseline)** | Empat percobaan ke aplikasi web MUSH Field Console: (a) SQL injection pada form login (`' OR 1=1 --`); (b) brute force `/api/auth/login` ke akun `operator` (gagal) lalu `admin` (berhasil, sandi `admin123`); (c) login pakai kredensial `operator/operator123` yang terpampang di halaman login; (d) `POST` data sensor palsu ke `/api/readings` tanpa token. | - Belum dikonfirmasi terpantau di Security Onion atau tidak (pengujian lewat localhost/172.26.80.1, bukan lewat Kali/Target Node di jaringan simulasi) | [Login_berhasil_kredensial_operator_terpampang.jpeg](redteam/evidence/Login_berhasil_kredensial_operator_terpampang.jpeg) |
| **Tahap 2 - Deteksi** | - | - Identifikasi log aplikasi untuk percobaan login beruntun dan payload SQLi, atau catat bila tidak ada mekanisme logging | [Payload_SQL_Injection_kolom_username_login.jpeg](redteam/evidence/Payload_SQL_Injection_kolom_username_login.jpeg) |
| **Tahap 3 - Analisis** | SQLi menyasar bypass login; brute force menyasar menebak kredensial admin; percobaan POST menguji apakah data sensor bisa dipalsukan. | SQLi gagal. Brute force berhasil karena tidak ada rate-limit di endpoint login dan sandi admin lemah. Kredensial demo operator/operator123 terekspos di halaman login | - |
| **Tahap 4 - Respons (Mitigasi)** | - | - Rate-limit/lockout pada /api/auth/login - Ganti sandi admin dan operator, hapus kredensial demo dari UI - Pertahankan kewajiban token pada /api/readings | [Uji_API_sensor_tanpa_token_GET_berhasil_POST_ditolak.jpeg](redteam/evidence/Uji_API_sensor_tanpa_token_GET_berhasil_POST_ditolak.jpeg) |
| **Tahap 5 - Verifikasi** | Ulangi brute force ke akun admin dan SQL injection yang sama | - Konfirmasi brute force kini diblokir dan kredensial demo tidak lagi tampil |  [Baseline_login_salah_kontrol_pembanding_SQLi.jpeg](redteam/evidence/Baseline_login_salah_kontrol_pembanding_SQLi.jpeg) |

## Skenario 2 dari 3 - Authentication Attack (Server Takeover)

- **Target:** Layanan OpenSSH, 192.168.14.5:22
- **Metode:** SSH Brute Force dan Credential Stuffing
- **Tools:** THC-Hydra / Ncrack + rockyou.txt
- **Tujuan:** Menebak sandi user server secara masif untuk mengambil alih sistem operasi.

| Tahap | Red Team | Blue Team | Bukti |
| :--- | :--- | :--- | :--- |
| **Tahap 1 - Serang (Baseline)** | Hydra (4 dan 10 task) dan Ncrack dari Kali (192.168.14.100) dengan user.txt + rockyou.txt ke ssh://192.168.14.5 | - Status deteksi awal di Security Onion: belum dikonfirmasi |  [Brute_force_login_API_berhasil_admin_admin123.jpeg](redteam/evidence/Brute_force_login_API_berhasil_admin_admin123.jpeg) |
| **Tahap 2 - Deteksi** | - | - Alert/log Security Onion untuk koneksi berulang ke port 22 dari 192.168.14.100: belum ada | [Alert_Security_Onion.jpeg](redteam/evidence/Hydra_dan_Ncrack_brute_force_SSH_ke_Target_Node_gagal.jpeg) |
| **Tahap 3 - Analisis** | Menebak sandi user server secara masif untuk mengambil alih SSH. | Server hanya menerima kunci publik sehingga wordlist tidak pernah tereksekusi; percobaan berikutnya berhenti karena *too many connection errors*, diduga rate-limit UFW (perlu konfirmasi lewat log) [Hydra_too_many_connection_errors_dan_hping3_tanpa_sudo_gagal.jpeg](redteam/evidence/Hydra_too_many_connection_errors_dan_hping3_tanpa_sudo_gagal.jpeg) | - |
| **Tahap 4 - Respons (Mitigasi)** | - | PermitRootLogin no; PasswordAuthentication no (key-only); ufw limit 22/tcp; LogLevel VERBOSE | [After_nonaktifkan_login_root.jpg](blueteam/evidence/After_nonaktifkan_login_root.jpg), [After_SSH_key-based_auth.jpg](blueteam/evidence/After_SSH_key-based_auth.jpg), [Before_dan_After_rate-limit_percobaan_login.jpg](blueteam/evidence/Before_dan_After_rate-limit_percobaan_login.jpg) |
| **Tahap 5 - Verifikasi** | Hydra dan Ncrack diulang terhadap target yang sudah ter-hardening | Gagal: tidak ada kredensial valid. Terdeteksi: belum dikonfirmasi | [Hydra_dan_Ncrack_brute_force_SSH_ke_Target_Node_gagal.jpeg](redteam/evidence/Hydra_dan_Ncrack_brute_force_SSH_ke_Target_Node_gagal.jpeg), [After_Kontrol_Access_Authentication_dan_Service_Attack_Surface.jpeg](blueteam/evidence/After_Kontrol_Access_Authentication_dan_Service_Attack_Surface.jpeg) |

Catatan: mitigasi sudah terpasang sebelum serangan dijalankan (Preparation), sehingga Tahap 4 di sini adalah baseline yang sudah ada.

## Skenario 3 dari 3 - Availability Attack (Service Disruption)

- **Target:** Infrastruktur jaringan Target Node, 192.168.14.5:80 (Apache2)
- **Metode:** TCP SYN Flood
- **Tools:** Hping3
- **Tujuan:** Membanjiri server dengan paket palsu hingga resource habis, menyebabkan web IoT down.

| Tahap | Red Team | Blue Team | Bukti |
| :--- | :--- | :--- | :--- |
| **Tahap 1 - Serang (Baseline)** | sudo hping3 -S -p 80 --flood -V 192.168.14.5 dari Kali (percobaan tanpa sudo gagal lebih dulu) | CPU target naik dari 7,8% menjadi 87,4%. Status deteksi awal: belum dikonfirmasi | [hping3_SYN_flood_port_80_ke_Target_Node.jpeg](redteam/evidence/hping3_SYN_flood_port_80_ke_Target_Node.jpeg), [Monitoring_SYN_flood_CPU_87_persen_htop.jpeg](redteam/evidence/Monitoring_SYN_flood_CPU_87_persen_htop.jpeg), [Baseline_monitoring_proses_dan_CPU_htop.jpeg](redteam/evidence/Baseline_monitoring_proses_dan_CPU_htop.jpeg) |
| **Tahap 2 - Deteksi** | - | Alert/lonjakan koneksi SYN di Security Onion: belum ada | [Monitoring_SYN_flood_CPU_87_persen_htop.jpeg](redteam/evidence/Monitoring_SYN_flood_CPU_87_persen_htop.jpeg) |
| **Tahap 3 - Analisis** | Membanjiri server dengan paket SYN palsu hingga resource habis. | Beban hampir seluruhnya softirq (pemrosesan paket kernel, user hanya 0,4%) [Monitoring_SYN_flood_softirq_87_persen_top.jpeg](redteam/evidence/Monitoring_SYN_flood_softirq_87_persen_top.jpeg). Port 80 hanya ALLOW tanpa batas laju koneksi. | - |
| **Tahap 4 - Respons (Mitigasi)** | - | net.ipv4.tcp_syncookies=1; ufw limit 80/tcp; rate-limit koneksi baru via iptables (hashlimit) | [Before_dan_After_rate-limit_percobaan_login.jpg](blueteam/evidence/Before_dan_After_rate-limit_percobaan_login.jpg) |
| **Tahap 5 - Verifikasi** | Ulangi skenario serangan yang SAMA setelah mitigasi | Serangan berhasil digagalkan | [Baseline_monitoring_proses_dan_CPU_htop.jpeg](redteam/evidence/Baseline_monitoring_proses_dan_CPU_htop.jpeg) |
