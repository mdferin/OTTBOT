# Bot OTT Telegram

Bot Telegram untuk menguruskan URL OTT (Online Television Tuner) dengan sistem langganan wajib ke channel Telegram.

## ðŸš€ Fungsi Utama

### 1. Sistem Langganan Wajib
- Memaksa pengguna langgan channel Telegram @onmyid sebelum boleh menggunakan bot
- Pengesahan secara langsung menggunakan Telegram API
- Butang untuk langgan channel dan pengesahan langganan

### 2. Pengurusan Akaun OTT
- `/start` - Mulakan bot dan akses menu utama
- `/info` - Lihat maklumat URL OTT anda
- `/resetdevice` - Reset peranti yang terdaftar

### 3. Sistem Redeem
- Butang "ðŸŽ Redeem URL OTT" untuk mendapatkan URL baru
- Sistem queue untuk mengurus permintaan redeem
- Tempoh percubaan percuma (10 hari)

### 4. Maklumat dan Harga
- Butang "ðŸ’µ Harga OTT" untuk melihat senarai harga
- Butang "â„¹ï¸ Info URL OTT" untuk melihat maklumat akaun

### 5. Sistem Renew
- Butang "ðŸ”„ Renew URL" untuk pengguna yang URL-nya telah expired

### 6. Notifikasi Automatik
- Notifikasi sebelum dan selepas URL expired (7, 3, 1 hari)
- Sistem penjadualan untuk menghantar notifikasi

## ðŸ”§ Fungsi Admin

### 1. Statistik dan Pemantauan
- `/stats` - Lihat statistik pengguna
- `/queue` - Lihat saiz antrian redeem
- `/ping` - Semak kesihatan sistem

### 2. Pengurusan Pengguna
- `/notify <user_id|all> <days>` - Hantar notifikasi manual
- `/delete <user_id>` - Padam pengguna
- `/resetnotify <user_id>` - Reset notifikasi user

### 3. Sistem Broadcast
- `/broadcast` - Hantar ke SEMUA pengguna (teks sahaja)
- `/broadcastto <user_id>` - Hantar ke SATU pengguna (teks sahaja)
- `/broadcastlist <user_id1> <user_id2> ...` - Hantar ke SENARAI pengguna (teks sahaja)

## âš™ï¸ Konfigurasi

### Tetapan Utama
- `BOT_TOKEN` - Token Telegram Bot
- `SAVE_PATH` - Lokasi penyimpanan fail pengguna
- `LOG_FILE` - Lokasi log redeem
- `BROADCAST_LOG_FILE` - Lokasi log broadcast
- `NOTIFICATION_DAYS` - Hari untuk menghantar notifikasi sebelum expired
- `ADMIN_ID` - ID Telegram admin
- `CHANNEL_USERNAME` - Username public channel Telegram yang wajib dilanggan

### Sistem Fail
- Fail per-UUID disimpan dalam direktori database
- Log sistem menggunakan format JSON
- Sistem lock fail untuk mengelakkan konflik

## ðŸ›¡ï¸ Keselamatan

### Pengesahan Langganan
- Pengguna mesti langgan channel @onmyid untuk menggunakan bot
- Pengesahan dilakukan secara langsung menggunakan Telegram API
- Semua fungsi bot memeriksa status langganan pengguna

### Rate Limiting
- Sistem retry untuk mengelakkan rate limit Telegram
- Penjadualan notifikasi untuk mengurangkan beban sistem

## ðŸ“Š Sistem Queue

### Redeem Queue
- Antrian untuk mengurus permintaan redeem yang serentak
- Worker sistem untuk memproses permintaan secara berurutan
- Status queue boleh dilihat oleh admin

## ðŸ“ Format Fail

### redeem_log.json
```json
{
  "user_id": {
    "nama": "Nama Pengguna",
    "uuid": "ID Unik",
    "redeem_time": timestamp,
    "expired_time": timestamp,
    "notified_days": [7, 3, 1]
  }
}
```

### Fail Per-UUID (database/uuid.json)
```json
{
  "expired_time": timestamp,
  "expired_desc": "Tarikh Expired",
  "limit_device": "1",
  "playlist_type": "all",
  "created_by": "DilarangBodoh"
}
```

## ðŸ¤– Command Handler

### Pengguna Biasa
- `/start` - Mulakan bot
- `/info` - Lihat maklumat
- `/resetdevice` - Reset peranti
- `/help` - Bantuan

### Admin
- `/stats` - Statistik pengguna
- `/notify` - Notifikasi manual
- `/delete` - Padam pengguna
- `/resetnotify` - Reset notifikasi
- `/ping` - Semak sistem
- `/queue` - Lihat queue
- `/broadcast` - Siar ke semua
- `/broadcastto` - Siar ke satu
- `/broadcastlist` - Siar ke senarai

## ðŸ”„ Callback Handler

### Butang Inline
- `redeem` - Redeem URL OTT
- `info` - Info URL OTT
- `harga` - Harga OTT
- `resetdevice` - Reset Device
- `renew` - Renew URL
- `check_subscription` - Semak langganan

## ðŸ“ˆ Sistem Notifikasi

### Automatik
- Dijalankan setiap 6 jam
- Memeriksa pengguna yang akan expired
- Menghantar notifikasi 7, 3, dan 1 hari sebelum expired
- Menghantar notifikasi apabila sudah expired

### Manual
- Admin boleh menghantar notifikasi pada bila-bila masa
- Boleh dihantar kepada semua pengguna atau pengguna tertentu
- Boleh menentukan hari sebelum/sudah expired

## ðŸ› ï¸ Fungsi Teknikal

### Utility Functions
- `load_redeem_log()` - Muat log redeem
- `safe_write()` - Tulis fail dengan selamat
- `save_redeem_log()` - Simpan log redeem
- `get_user_name()` - Dapatkan nama pengguna
- `short_id()` - Jana ID unik
- `is_expired()` - Semak status expired
- `safe_send()` - Hantar mesej dengan retry
- `alert_admin()` - Hantar alert ke admin
- `load_broadcast_log()` - Muat log broadcast
- `save_broadcast_log()` - Simpan log broadcast
- `generate_message_hash()` - Jana hash mesej
- `is_user_subscribed()` - Semak langganan pengguna

### Worker Functions
- `redeem_worker()` - Proses queue redeem
- `notification_task()` - Tugas penjadualan notifikasi

### Broadcast Functions
- `broadcast()` - Siar ke semua pengguna
- `broadcastto()` - Siar ke satu pengguna
- `broadcastlist()` - Siar ke senarai pengguna
- `handle_broadcast_confirmation()` - Kendali pengesahan broadcast

## ðŸ” Keselamatan Data

### Perlindungan Rate Limit
- Retry automatik untuk ralat network
- Jeda antara penghantaran untuk mengelakkan rate limit
- Penjadualan tugas background

### Pengurusan Ralat
- Logging ralat terperinci
- Alert automatik ke admin apabila ada ralat kritikal
- Pengendalian exception untuk semua fungsi utama

## ðŸ“¦ Dependensi

### Pustaka Utama
- `python-telegram-bot` - API Telegram
- `pytz` - Pengurusan zon masa
- `asyncio` - Asynchronous programming
- `json` - Pengurusan data format JSON
- `logging` - Sistem logging
- `os` - Interaksi sistem fail
- `random` & `string` - Jana ID rawak

### Versi Minimum
- Python 3.7+
- python-telegram-bot 20.0+

## ðŸš€ Deployment

### Keperluan Sistem
- Server Linux/Windows dengan Python 3.7+
- Akses internet untuk sambungan Telegram API
- Storan mencukupi untuk fail database
- RAM mencukupi untuk operasi asynchronous

### Langkah Deployment
1. Pasang dependensi: `pip install python-telegram-bot`
2. Tetapkan konfigurasi dalam kod (token bot, path, dll)
3. Jalankan bot: `python bot.py`
4. Pastikan direktori database boleh ditulis

### Penyelenggaraan
- Monitor log sistem untuk ralat
- Backup fail database secara berkala
- Update token bot jika perlu
- Monitor penggunaan sumber server
