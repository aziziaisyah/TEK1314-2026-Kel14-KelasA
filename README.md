# PBL Keamanan Siber - Kelompok 14
## Pengamanan Sistem IoT MUSH: Simulasi Serangan & Mitigasi Keamanan Siber

Proyek ini mereplikasi sisi server dari sistem IoT **MUSH** (Made Up Slowly Here) —
sistem pemantauan fermentasi silase berbasis ESP32, MQTT, dan dashboard React —
untuk disimulasikan sebagai target dalam latihan Red Team vs Blue Team (Purple Team Loop).

**Tim:**
| Nama | NIM | Peran |
|---|---|---|
| Aisyah Elfiyuni Mangkuto Azizi | J0404241067 | Lead |
| Adnan Muhammad Gania Kamaludin | J0404241098 | Red Team |
| Merlanza Rizki Tumangger | J0404241062 | Blue Team |
| Muhammad Dzaky Faizal | J0404241138 | Blue Team |

## Arsitektur Simulasi

| Hostname | IP Address | OS |
|---|---|---|
| Target Server | 192.168.14.5 | Ubuntu Server |
| Attacker Node | 192.168.14.100 | Kali Linux |
| Monitoring Node | 192.168.14.200 | Security Onion |

## Skenario Serangan yang Disimulasikan

Tiga skenario diuji pada Fase 2 (Attack Plan):

1. **Authentication Attack - SSH Brute Force & Credential Stuffing**
   Target: layanan OpenSSH (port 22). Tools: THC-Hydra / Ncrack + wordlist `rockyou.txt`.
   Hasil: **digagalkan sepenuhnya** oleh hardening baseline (key-only auth + rate-limit UFW).

2. **Availability Attack - TCP SYN Flood**
   Target: infrastruktur jaringan Target Node, port 80. Tools: `hping3`.
   Hasil: **berhasil dieksekusi**, CPU naik dari 7,8% menjadi 87,4% (dominan softirq);
   mitigasi rate-limit & SYN cookies telah diterapkan.

3. **Web Application Attack - SQL Injection**
   Target: dasbor web IoT (MUSH Field Console). Tools: payload manual / SQLMap.
   Hasil: SQL Injection klasik **gagal** menembus form login, namun ditemukan celah
   kredensial lemah (default admin/operator) dan endpoint API tanpa autentikasi;
   mitigasi telah diterapkan.

Detail lengkap metodologi, bukti serangan, deteksi, mitigasi, dan verifikasi
tersedia di **Incident Report Final** dan **Purple Team Log**.

## Perjalanan Proyek (Fase 1-3)

| Fase | Deskripsi | Dokumen |
|---|---|---|
| Fase 1 - Desain & Replika | Topologi, IP plan, dan pemetaan tech stack sistem IoT asli ke replika | [docs/design/topology.png](docs/design/topology.png), [docs/design/ip_plan.md](docs/design/ip_plan.md), [tech_stack_declaration.md](docs/presentation-minggu9/tech_stack_declaration.md) |
| Fase 2 - Hardening & Serangan | Hardening baseline, tiga skenario attack plan, Purple Team Loop, forensik, dan remediasi | [docs/redteam/attack_plan.md](docs/redteam/attack_plan.md), [docs/blueteam/hardening_checklist.md](docs/blueteam/hardening_checklist.md), [docs/purple-team-log.md](docs/purple-team-log.md) |
| Fase 3 - Penutupan Siklus | Incident Report final, video demo, presentasi, dan artefak GitHub | [docs/Incident_Report_Final.pdf](docs/Incident_Report_Final.pdf), [docs/presentation-final/slides.pdf](docs/presentation-final/slides.pdf) |

## Bukti Pendukung

- Evidence Red Team: [docs/redteam/evidence/](docs/redteam/evidence/)
- Evidence Blue Team: [docs/blueteam/evidence/](docs/blueteam/evidence/)

## Video Demo

[Video Demo PBL Kelompok 14](https://drive.google.com/file/d/12aVfdowIMZJkzP8WI2TKRFFZw1qhieQM/view?usp=sharing)

## Pembelajaran Utama

- Autentikasi kunci publik menutup brute force SSH secara tuntas.
- Rate-limit per layanan tidak otomatis melindungi layanan lain (port 22 dibatasi,
  port 80 tetap rentan terhadap SYN flood).
- Kontrol akses API perlu konsisten di seluruh endpoint, tidak hanya pada endpoint
  yang memodifikasi data.
