# Hermes Agent — Skills, Memories & Config Backup

Backup otomatis skills, memories, dan config Hermes Agent ke GitHub.

---

## 🚨 Emergency Restore ke Server Baru

### 1. Install Hermes Agent
```bash
pip install hermes-agent
# atau
curl -sSL https://hermes-agent.nousresearch.com/install.sh | bash
```

### 2. Clone backup ini
```bash
git clone https://GITHUB_TOKEN@github.com/arkhindi12/vps-hermes.git /tmp/hermes-restore
cd /tmp/hermes-restore
```

### 3. Extract backup terbaru
```bash
# Cari file terbaru
ls -t backup-*.tar.gz | head -1

# Extract
tar xzf backup-YYYY-MM-DD-HH-MM-SS.tar.gz
cd harnet-backup-hermes-*/
```

### 4. Restore skills
```bash
# Backup dulu skills yang ada (kalau ada)
cp -r ~/.hermes/skills ~/.hermes/skills.bak 2>/dev/null

# Restore dari backup
tar xzf extra-0-skills.tar.gz -C ~/.hermes/
```

### 5. Restore memories
```bash
tar xzf extra-1-memories.tar.gz -C ~/.hermes/
```

### 6. Restore scripts (optional)
```bash
tar xzf extra-2-scripts.tar.gz -C ~/.hermes/ 2>/dev/null || true
```

### 7. Restore config (optional)
```bash
# Config ada di app.tar.gz (root ~/.hermes dir)
tar xzf app.tar.gz -C ~/.hermes/ --wildcards '*.yaml' '*.json' 2>/dev/null || true
```

### 8. Restart Hermes
```bash
# CLI
hermes restart

# atau PM2 (jika dijalankan via PM2)
pm2 restart hermes
```

---

## ⏰ Auto-backup Schedule

Backup dijadwalkan **tiap hari jam 02:00** via Harnet Panel.

Yang di-backup:
| Item | Path |
|---|---|
| Skills (SKILL.md files) | `~/.hermes/skills/` |
| Memories | `~/.hermes/memories/MEMORY.md`, `USER.md` |
| Scripts | `~/.hermes/scripts/` |
| Full .hermes dir | `~/.hermes/` (app.tar.gz) |

---

## 📋 Isi Backup

Setiap backup berisi:
- `app.tar.gz` — full `~/.hermes/` (kecuali sessions/logs/cache)
- `extra-0-skills.tar.gz` — skills directory saja
- `extra-1-memories.tar.gz` — memories (MEMORY.md, USER.md)
- `extra-2-scripts.tar.gz` — custom scripts

Disimpan max 5 backup terakhir.

---

## 🔗 Related Repos

| Repo | Isi |
|---|---|
| arkhindi12/vps-web-manager | Harnet Panel (VPS control panel) |
| arkhindi12/vps-hermes | Hermes skills + memories (this repo) |
| arkhindi12/vps-9router | 9Router app |
| arkhindi12/vps-mailserver | Mail server config |
