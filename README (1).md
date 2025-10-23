# 🎯 Sistema TKI Completo con Database

Sistema completo per la somministrazione del Thomas-Kilmann Conflict Mode Instrument con:
- ✅ Autenticazione utenti con codice anonimo
- ✅ Questionario demografico iniziale
- ✅ Salvataggio risultati in database SQLite
- ✅ Dashboard admin per analisi aggregate
- ✅ Export dati CSV
- ✅ Sistema di sessioni con JWT

---

## 📋 Requisiti

- **Node.js** 16+ 
- **npm** 8+

---

## 🚀 Installazione e Avvio

### 1. Installare Dipendenze

```bash
cd backend
npm install
```

### 2. Inizializzare Database

```bash
npm run init-db
```

Questo creerà:
- Database SQLite in `backend/database/tki.db`
- Tabelle: `admin`, `users`, `profiles`, `results`, `sessions`
- Admin di default: 
  - Username: `admin`
  - Password: `AdminPassword123!` (⚠️ **CAMBIARE IN PRODUZIONE**)
- 3 utenti di test: `TEST001`, `TEST002`, `TEST003` (password: `Test123!`)

### 3. Configurare Variabili d'Ambiente

Modifica il file `.env` se necessario:

```env
PORT=3000
JWT_SECRET=your-secret-key-change-this-in-production
ADMIN_USERNAME=admin
ADMIN_PASSWORD=AdminPassword123!
CORS_ORIGIN=http://localhost:8080
```

⚠️ **IMPORTANTE:** In produzione, cambiare:
- `JWT_SECRET` con una stringa random lunga
- `ADMIN_PASSWORD` con una password sicura
- `CORS_ORIGIN` con il dominio reale

### 4. Avviare Server

```bash
npm start
```

oppure in modalità development (con auto-reload):

```bash
npm run dev
```

Il server sarà disponibile su: `http://localhost:3000`

---

## 📡 API Endpoints

### 🔐 Autenticazione

#### Registrazione Utente
```http
POST /api/auth/register
Content-Type: application/json

{
  "password": "UserPassword123!",
  "ageRange": "25-34",
  "gender": "M/F/Altro",
  "educationLevel": "Laurea",
  "workExperience": "5-10 anni",
  "industry": "Tecnologia",
  "jobRole": "Manager",
  "organizationSize": "50-100",
  "additionalNotes": "Note opzionali"
}

Response:
{
  "success": true,
  "message": "Registrazione completata con successo",
  "userCode": "TKI-ABCD-EFGH",
  "token": "jwt_token_here",
  "userId": 1
}
```

**Nota:** Il sistema genera automaticamente un codice utente univoco (formato: `TKI-XXXX-XXXX`)

#### Login Utente
```http
POST /api/auth/login
Content-Type: application/json

{
  "userCode": "TKI-ABCD-EFGH",
  "password": "UserPassword123!"
}

Response:
{
  "success": true,
  "token": "jwt_token_here",
  "userId": 1,
  "userCode": "TKI-ABCD-EFGH",
  "completed": false
}
```

#### Login Admin
```http
POST /api/auth/admin/login
Content-Type: application/json

{
  "username": "admin",
  "password": "AdminPassword123!"
}

Response:
{
  "success": true,
  "token": "jwt_token_here",
  "adminId": 1,
  "username": "admin"
}
```

### 📊 Risultati (Autenticati)

#### Salvare Risultati Test
```http
POST /api/results/save
Authorization: Bearer {token}
Content-Type: application/json

{
  "answers": {"1": "a", "2": "b", ...},
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
    ...
  },
  "ranges": {
    "competing": "MEDIO",
    "collaborating": "ALTO",
    ...
  },
  "dominantMode": "collaborating",
  "notes": "Note opzionali"
}
```

#### Recuperare Risultati Utente
```http
GET /api/results/user
Authorization: Bearer {token}
```

#### Recuperare Profilo Utente
```http
GET /api/profile
Authorization: Bearer {token}
```

### 👨‍💼 Admin (Solo Admin)

#### Lista Utenti
```http
GET /api/admin/users
Authorization: Bearer {admin_token}
```

#### Tutti i Risultati
```http
GET /api/admin/results
Authorization: Bearer {admin_token}
```

#### Statistiche Aggregate
```http
GET /api/admin/stats
Authorization: Bearer {admin_token}
```

Ritorna:
- Numero totale utenti
- Test completati
- Punteggi medi per modalità
- Distribuzione modalità dominanti
- Statistiche per genere
- Statistiche per settore

#### Export CSV
```http
GET /api/admin/export/csv
Authorization: Bearer {admin_token}
```

Scarica file CSV con tutti i risultati.

### 🏥 Health Check

```http
GET /api/health
```

---

## 🗄️ Schema Database

### Tabella `users`
- `id` - Primary key
- `user_code` - Codice univoco (es. TKI-ABCD-EFGH)
- `password_hash` - Password hashata con bcrypt
- `created_at` - Data registrazione
- `last_access` - Ultimo accesso
- `status` - Stato account (active/suspended)
- `completed` - Test completato (boolean)

### Tabella `profiles`
- `id` - Primary key
- `user_id` - Foreign key → users
- `age_range` - Fascia età
- `gender` - Genere
- `education_level` - Livello istruzione
- `work_experience` - Anni esperienza
- `industry` - Settore
- `job_role` - Ruolo lavorativo
- `organization_size` - Dimensione organizzazione
- `additional_notes` - Note aggiuntive

### Tabella `results`
- `id` - Primary key
- `user_id` - Foreign key → users
- `test_date` - Data test
- `answers` - JSON risposte (30 domande)
- `competing_score` - Punteggio Competizione (0-12)
- `collaborating_score` - Punteggio Collaborazione (0-12)
- `compromising_score` - Punteggio Compromesso (0-12)
- `avoiding_score` - Punteggio Elusione (0-12)
- `accommodating_score` - Punteggio Accomodamento (0-12)
- `*_percentile` - Percentili per ogni modalità
- `*_range` - Range (ALTO/MEDIO/BASSO)
- `dominant_mode` - Modalità dominante

### Tabella `admin`
- `id` - Primary key
- `username` - Username admin
- `password_hash` - Password hashata
- `email` - Email
- `created_at` - Data creazione
- `last_login` - Ultimo login

### Tabella `sessions`
- `id` - Primary key
- `user_id` - Foreign key → users
- `user_type` - Tipo utente (user/admin)
- `login_time` - Timestamp login
- `ip_address` - IP address
- `user_agent` - User agent

---

## 🔒 Sicurezza

### Password
- Hashate con **bcrypt** (10 rounds)
- Requisito minimo: 6 caratteri
- In produzione: implementare requisiti più stringenti

### JWT
- Token firmati con `JWT_SECRET`
- Durata default: 7 giorni
- Includono: `id`, `code`/`username`, `type` (user/admin)

### CORS
- Configurabile via `CORS_ORIGIN`
- Default: permette tutte le origini (*)
- In produzione: limitare al dominio specifico

### SQL Injection
- Protezione nativa tramite prepared statements di SQLite3

---

## 📊 Flusso Utente

1. **Registrazione**
   - Utente compila questionario demografico
   - Sistema genera codice univoco (TKI-XXXX-XXXX)
   - Utente imposta password
   - Riceve token JWT

2. **Test TKI**
   - Risponde alle 30 domande
   - Frontend calcola punteggi
   - Salva risultati via API

3. **Visualizzazione Risultati**
   - Accede con codice utente + password
   - Visualizza i suoi risultati storici
   - Può rifare il test

4. **Admin**
   - Login con credenziali admin
   - Visualizza dashboard con statistiche
   - Export dati per analisi

---

## 📈 Statistiche Disponibili (Admin)

- **Totale utenti registrati**
- **Test completati**
- **Punteggi medi** per ogni modalità TKI
- **Distribuzione modalità dominanti**
- **Analisi per genere**
- **Analisi per settore**
- **Analisi per livello istruzione**
- **Timeline registrazioni**

---

## 🔧 Manutenzione

### Backup Database

```bash
# Copia database
cp backend/database/tki.db backend/database/tki_backup_$(date +%Y%m%d).db
```

### Reset Database

```bash
# Elimina e ricrea database
rm backend/database/tki.db
npm run init-db
```

### Cambio Password Admin

```javascript
// Usa Node.js REPL
const bcrypt = require('bcryptjs');
const hash = bcrypt.hashSync('NuovaPassword123!', 10);
console.log(hash);

// Poi aggiorna manualmente nel database
sqlite3 backend/database/tki.db
UPDATE admin SET password_hash = 'hash_generato' WHERE username = 'admin';
```

---

## 🐛 Troubleshooting

### Errore: "Cannot find module"
```bash
npm install
```

### Errore: "Database locked"
- Chiudi tutte le connessioni al database
- Riavvia il server

### Errore: "JWT expired"
- Token scaduto
- L'utente deve rifare login

### Port già in uso
- Cambia `PORT` in `.env`
- Oppure: `PORT=3001 npm start`

---

## 🚀 Deploy Produzione

### 1. Variabili d'Ambiente
```bash
# .env produzione
NODE_ENV=production
PORT=3000
JWT_SECRET=your-very-long-random-secret-string-change-this
ADMIN_PASSWORD=SecureAdminPassword!
CORS_ORIGIN=https://yourdomain.com
DATABASE_PATH=./database/tki.db
```

### 2. PM2 (Process Manager)
```bash
npm install -g pm2
pm2 start server.js --name tki-backend
pm2 save
pm2 startup
```

### 3. Nginx Reverse Proxy
```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location /api {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### 4. HTTPS con Let's Encrypt
```bash
sudo apt-get install certbot python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com
```

---

## 📝 TODO / Roadmap

- [ ] Sistema di recupero password
- [ ] Email notifiche
- [ ] Dashboard grafiche più avanzate
- [ ] Gestione Team/Organizzazioni
- [ ] Report PDF automatici
- [ ] API rate limiting
- [ ] Logging avanzato
- [ ] Backup automatici
- [ ] Supporto PostgreSQL
- [ ] Docker containerization

---

## 📞 Supporto

Per problemi o domande:
- Consulta questa documentazione
- Controlla i log del server
- Verifica lo stato del database

---

## 📄 Licenza

MIT License

---

**Versione:** 1.0.0  
**Data:** Ottobre 2025  
**Autore:** Sistema TKI Team
