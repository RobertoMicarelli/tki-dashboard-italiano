# 🚀 GUIDA RAPIDA - Sistema TKI Completo

## ⚡ Avvio Veloce (5 minuti)

### 1️⃣ Installa Node.js
Se non hai Node.js installato:
- Vai su https://nodejs.org
- Scarica la versione LTS
- Installa seguendo le istruzioni

Verifica l'installazione:
```bash
node --version
npm --version
```

### 2️⃣ Configura il Sistema

```bash
# 1. Apri terminale nella cartella backend
cd tki-system-complete/backend

# 2. Installa dipendenze (ci vogliono 1-2 minuti)
npm install

# 3. Inizializza database
npm run init-db

# 4. Avvia server
npm start
```

✅ **Il server è pronto quando vedi:**
```
🚀 =======================================
✅ Server TKI in esecuzione su porta 3000
```

### 3️⃣ Testa il Sistema

Apri un nuovo terminale e testa:

```bash
# Test health check
curl http://localhost:3000/api/health

# Dovresti vedere: {"status":"ok","database":"connected",...}
```

### 4️⃣ Login Admin di Test

Credenziali di default:
- **Username:** `admin`
- **Password:** `AdminPassword123!`

Testa il login admin:
```bash
curl -X POST http://localhost:3000/api/auth/admin/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"AdminPassword123!"}'
```

Dovresti ricevere un token JWT!

### 5️⃣ Test con Utente di Prova

Sono stati creati 3 utenti di test:
- **Codice:** `TEST001` - **Password:** `Test123!`
- **Codice:** `TEST002` - **Password:** `Test123!`
- **Codice:** `TEST003` - **Password:** `Test123!`

Testa login utente:
```bash
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"userCode":"TEST001","password":"Test123!"}'
```

---

## 🎯 Prossimi Passi

### Opzione A: Usa l'API direttamente
Consulta il README.md per tutti gli endpoints disponibili

### Opzione B: Integra con Frontend
1. Apri il file `TKI_Dashboard_Italiano.html` 
2. Modifica la sezione API per connetterla al backend
3. Implementa le chiamate API per registrazione, login e salvataggio

### Opzione C: Usa Postman/Insomnia
1. Scarica Postman (https://www.postman.com/)
2. Importa gli endpoints dal README
3. Testa tutte le API

---

## 📊 Struttura del Flusso

```
┌─────────────────────────────────────────────┐
│  1. REGISTRAZIONE UTENTE                     │
│     POST /api/auth/register                  │
│     • Questionario demografico               │
│     • Password                               │
│     → Riceve: userCode + token              │
└─────────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────────┐
│  2. COMPILAZIONE TEST TKI                    │
│     • 30 domande                             │
│     • Calcolo punteggi                       │
│     POST /api/results/save                   │
│     Authorization: Bearer {token}            │
└─────────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────────┐
│  3. VISUALIZZAZIONE RISULTATI                │
│     GET /api/results/user                    │
│     GET /api/profile                         │
│     Authorization: Bearer {token}            │
└─────────────────────────────────────────────┘

    ┌──────────────────────────────────────┐
    │  ADMIN: Accesso Completo             │
    │  POST /api/auth/admin/login          │
    │  • GET /api/admin/users              │
    │  • GET /api/admin/results            │
    │  • GET /api/admin/stats              │
    │  • GET /api/admin/export/csv         │
    └──────────────────────────────────────┘
```

---

## 🔑 Esempio Completo con CURL

### 1. Registrazione Nuovo Utente
```bash
curl -X POST http://localhost:3000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "password": "MyPassword123!",
    "ageRange": "25-34",
    "gender": "M",
    "educationLevel": "Laurea Magistrale",
    "workExperience": "5-10 anni",
    "industry": "Tecnologia",
    "jobRole": "Manager",
    "organizationSize": "50-100",
    "additionalNotes": "Test utente"
  }'
```

**Risposta:**
```json
{
  "success": true,
  "message": "Registrazione completata con successo",
  "userCode": "TKI-AB12-CD34",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "userId": 4
}
```

**💾 SALVA:** `userCode` e `token`!

### 2. Salvare Risultati Test
```bash
TOKEN="il_tuo_token_qui"

curl -X POST http://localhost:3000/api/results/save \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{
    "answers": {"1":"a","2":"b","3":"a",...},
    "scores": {
      "competing": 5,
      "collaborating": 8,
      "compromising": 6,
      "avoiding": 4,
      "accommodating": 7
    },
    "percentiles": {
      "competing": 50,
      "collaborating": 80,
      "compromising": 60,
      "avoiding": 40,
      "accommodating": 70
    },
    "ranges": {
      "competing": "MEDIO",
      "collaborating": "ALTO",
      "compromising": "MEDIO",
      "avoiding": "BASSO",
      "accommodating": "MEDIO"
    },
    "dominantMode": "collaborating"
  }'
```

### 3. Recuperare Risultati
```bash
curl -X GET http://localhost:3000/api/results/user \
  -H "Authorization: Bearer $TOKEN"
```

### 4. Admin: Statistiche
```bash
ADMIN_TOKEN="token_admin_qui"

curl -X GET http://localhost:3000/api/admin/stats \
  -H "Authorization: Bearer $ADMIN_TOKEN"
```

### 5. Admin: Export CSV
```bash
curl -X GET http://localhost:3000/api/admin/export/csv \
  -H "Authorization: Bearer $ADMIN_TOKEN" \
  --output risultati-tki.csv
```

---

## 🛠️ Comandi Utili

### Fermare il Server
```bash
# Nel terminale dove gira il server
Ctrl + C
```

### Riavviare il Server
```bash
npm start
```

### Vedere il Database
```bash
# Installa sqlite3 se non l'hai
npm install -g sqlite3

# Apri il database
sqlite3 backend/database/tki.db

# Comandi SQL
.tables                           # Lista tabelle
SELECT * FROM users;              # Vedi tutti gli utenti
SELECT * FROM results;            # Vedi tutti i risultati
.exit                             # Esci
```

### Reset Completo
```bash
# Elimina database
rm backend/database/tki.db

# Ricrea tutto
npm run init-db
```

---

## ❓ FAQ

**Q: Il server non parte**
```bash
# Verifica che la porta 3000 sia libera
lsof -i :3000

# Usa altra porta
PORT=3001 npm start
```

**Q: Errore "Cannot find module"**
```bash
# Reinstalla dipendenze
rm -rf node_modules
npm install
```

**Q: Come cambio la password admin?**
- Modifica il file `.env`
- Elimina e ricrea il database con `npm run init-db`

**Q: Posso usare PostgreSQL invece di SQLite?**
- Sì! Dovrai modificare il codice del database
- SQLite è perfetto per < 10.000 utenti

**Q: Come metto in produzione?**
- Consulta la sezione "Deploy Produzione" nel README.md
- Usa PM2 per il process management
- Configura Nginx come reverse proxy
- Usa HTTPS con Let's Encrypt

---

## 📞 Supporto

**File di log:**
- Il server stampa tutti gli errori nel terminale
- Controlla i messaggi di errore per debugging

**Database:**
- File: `backend/database/tki.db`
- Puoi ispezionarlo con sqlite3 o DB Browser for SQLite

**Problemi comuni:**
1. Porta già in uso → Usa altra porta
2. Database locked → Chiudi tutte le connessioni
3. JWT expired → Rifare login

---

## ✅ Checklist di Verifica

- [ ] Node.js installato (v16+)
- [ ] Dipendenze installate (`npm install`)
- [ ] Database creato (`npm run init-db`)
- [ ] Server in esecuzione (`npm start`)
- [ ] Health check OK (`curl localhost:3000/api/health`)
- [ ] Login admin funziona
- [ ] Login utente test funziona

**Tutto OK?** 🎉 Il sistema è pronto!

---

**Versione:** 1.0.0  
**Ultimo aggiornamento:** Ottobre 2025
