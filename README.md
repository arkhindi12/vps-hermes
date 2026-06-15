# 🤖 Hermes Agent — Skills, Memories & Config Backup

> Ini adalah backup otomatis dari **Hermes Agent** yang berjalan di VPS `web.arzastore.com`.
> Berisi koleksi skills, memories (preferensi user), dan config yang dibangun selama penggunaan.

Hermes Agent adalah AI assistant berbasis LLM yang berjalan lokal di server, bisa dihubungkan ke Telegram, menjalankan perintah terminal, mengelola file, backup otomatis, dan banyak lagi.

---

## 📦 Isi Repo Ini

Setiap backup berisi:

| File | Keterangan |
|---|---|
| `extra-0-skills.tar.gz` | Semua skill (SKILL.md) — cara Hermes mengerjakan tugas-tugas spesifik |
| `extra-1-memories.tar.gz` | Memories — preferensi user, konteks lingkungan, fakta penting |
| `extra-2-scripts.tar.gz` | Custom scripts yang dipanggil oleh skill/cron |
| `app.tar.gz` | Full `~/.hermes/` directory (config, cron, plugins) |

Skills mencakup:
- **VPS ops** — deploy, backup, nginx, SSL, docker, PM2
- **AI tools** — vLLM, llama.cpp, DSPy, HuggingFace
- **Hermes config** — setup provider, gateway Telegram, failover
- **Dev workflows** — TDD, code review, debugging, planning
- **Media** — YouTube download, ClipSense, audio/video processing
- **Dan 50+ skill lainnya**

---

## 🚨 Emergency Restore ke Server Baru

Ketika VPS mati total, semua file hilang, atau pindah server:

### Step 1 — Install Hermes Agent
```bash
pip install hermes-agent
# atau ikuti docs: https://hermes-agent.nousresearch.com/docs
```

### Step 2 — Clone backup ini
```bash
git clone https://GITHUB_TOKEN@github.com/arkhindi12/vps-hermes.git /tmp/hermes-restore
cd /tmp/hermes-restore
```

### Step 3 — Extract backup terbaru
```bash
# Lihat file terbaru
ls -t backup-*.tar.gz | head -3

# Extract (ganti nama file sesuai yang terbaru)
tar xzf backup-YYYY-MM-DD-HH-MM-SS.tar.gz
cd harnet-backup-hermes-*/
```

### Step 4 — Restore skills
```bash
mkdir -p ~/.hermes/skills
tar xzf extra-0-skills.tar.gz -C ~/.hermes/
echo "✅ Skills restored"
```

### Step 5 — Restore memories
```bash
tar xzf extra-1-memories.tar.gz -C ~/.hermes/
echo "✅ Memories restored"
```

### Step 6 — Restore scripts (optional)
```bash
tar xzf extra-2-scripts.tar.gz -C ~/.hermes/ 2>/dev/null && echo "✅ Scripts restored"
```

### Step 7 — Restore config
```bash
# Extract config files saja dari app.tar.gz
tar xzf app.tar.gz -C ~/.hermes/ --wildcards 'config.yaml' '*.json' 2>/dev/null || true
```

### Step 8 — Restart Hermes
```bash
pm2 restart hermes
# atau
hermes --profile default
```

---

## ⏰ Auto-backup Schedule

Backup berjalan **otomatis setiap hari jam 02:00** via Harnet Panel (`web.arzastore.com/backup`).

- Max 5 backup tersimpan (otomatis hapus yang lama)
- Push ke repo ini via GitHub API

---

## 🔗 Ekosistem VPS

Repo ini bagian dari sistem backup terpusat di Harnet Panel:

| Repo | Isi | Auto-backup |
|---|---|---|
| [vps-web-manager](https://github.com/arkhindi12/vps-web-manager) | Harnet Panel source | ✅ 02:00 |
| [vps-hermes](https://github.com/arkhindi12/vps-hermes) | Hermes skills + memories | ✅ 02:00 |
| [vps-9router](https://github.com/arkhindi12/vps-9router) | 9Router VPN app | ✅ 02:00 |
| [vps-vpn-status](https://github.com/arkhindi12/vps-vpn-status) | VPN status monitor | ✅ 02:00 |
| [vps-adminvpn-agent](https://github.com/arkhindi12/vps-adminvpn-agent) | Admin VPN agent | ✅ 02:00 |
| [vps-adminvpn-control](https://github.com/arkhindi12/vps-adminvpn-control) | Admin VPN control | ✅ 02:00 |
| [vps-winnerlens](https://github.com/arkhindi12/vps-winnerlens) | Winnerlens app | ✅ 02:00 |
| [vps-mailserver](https://github.com/arkhindi12/vps-mailserver) | Mail server config | ✅ 03:00 |

---

## 📞 Info Server

- **Panel:** https://web.arzastore.com
- **Hermes via Telegram:** @zaguza
- **Backup dikelola oleh:** [Harnet Panel](https://web.arzastore.com) — VPS control panel custom berbasis Node.js
