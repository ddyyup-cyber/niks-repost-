# WormGPT Admin Telegram Bot

## Cara pakai (Termux / Pterodactyl)

1. Buka file `bot.js`
2. Isi bagian **CONFIG** di atas:
   - `TELEGRAM_BOT_TOKEN` → dari @BotFather
   - `ADMIN_IDS` → user id kamu (cek di @userinfobot), contoh `[712345678]`
   - `SUPABASE_URL` → URL project Supabase
   - `SUPABASE_SERVICE_ROLE_KEY` → **service_role** (bukan publishable)

3. Install & jalankan:

```bash
cd telegram-bot
npm install
node bot.js
```

### Pterodactyl
- Upload folder `telegram-bot`
- Startup command: `node bot.js`
- Tidak perlu isi Environment Variables (sudah di script)

### Termux
```bash
pkg install nodejs
cd telegram-bot
npm install
node bot.js
```

## Tombol bot
- **Generate Token** → ketik jumlah limit
- **List Token Aktif**
- **Statistik**
- **Set Free Limit** → free limit user baru

## Catatan key
- Jangan pakai key `sb_publishable_...`
- Pakai **service_role** (biasanya diawali `eyJ...`)
