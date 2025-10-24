# 📄 ISTRUZIONI PER CREARE IL TEMPLATE GOOGLE DOCS

## 🎯 COME IMPORTARE IL TEMPLATE

### Metodo 1: Importazione Diretta (Raccomandato)
1. **Apri Google Docs** (docs.google.com)
2. **Clicca su "Nuovo"** → "Carica file"
3. **Seleziona** `Template_TKI_Report_Google_Docs.html`
4. **Google Docs** convertirà automaticamente l'HTML in un documento
5. **Salva** come "Template TKI Report"

### Metodo 2: Copia e Incolla
1. **Apri** `Template_TKI_Report_Google_Docs.html` in un browser
2. **Seleziona tutto** (Ctrl+A / Cmd+A)
3. **Copia** (Ctrl+C / Cmd+C)
4. **Apri Google Docs** e crea un nuovo documento
5. **Incolla** (Ctrl+V / Cmd+V)
6. **Salva** come "Template TKI Report"

## 🔧 PERSONALIZZAZIONE DEL TEMPLATE

### 1. Logo Aziendale
- **Sostituisci** `[LOGO AZIENDALE]` con il tuo logo
- **Dimensioni consigliate:** 200x100px
- **Formato:** PNG o JPG

### 2. Colori Aziendali
- **Modifica** i colori CSS nelle sezioni:
  - `.competing { background: #E74C3C; }`
  - `.collaborating { background: #27AE60; }`
  - `.compromising { background: #3498DB; }`
  - `.avoiding { background: #95A5A6; }`
  - `.accommodating { background: #9B59B6; }`

### 3. Informazioni Aziendali
- **Sostituisci** "Dr. Roberto Micarelli" con i tuoi dati
- **Aggiorna** "Connectance ETS & TopForGrowth" con la tua azienda
- **Modifica** l'anno "2025" se necessario

## 📊 PLACEHOLDER PER AUTOMAZIONE

Il template contiene questi placeholder che verranno sostituiti automaticamente:

### Informazioni Utente
- `{{NOME_UTENTE}}` - Nome del partecipante
- `{{DATA_GENERAZIONE}}` - Data di creazione del report
- `{{CODICE_REPORT}}` - Codice univoco del report

### Punteggi e Percentili
- `{{PUNTEGGIO_COMPETIZIONE}}` - Punteggio grezzo (0-12)
- `{{PERCENTILE_COMPETIZIONE}}` - Percentile (0-100)
- `{{RANGE_COMPETIZIONE}}` - ALTO/MEDIO/BASSO
- `{{PUNTEGGIO_COLLABORAZIONE}}` - Punteggio grezzo (0-12)
- `{{PERCENTILE_COLLABORAZIONE}}` - Percentile (0-100)
- `{{RANGE_COLLABORAZIONE}}` - ALTO/MEDIO/BASSO
- `{{PUNTEGGIO_COMPROMESSO}}` - Punteggio grezzo (0-12)
- `{{PERCENTILE_COMPROMESSO}}` - Percentile (0-100)
- `{{RANGE_COMPROMESSO}}` - ALTO/MEDIO/BASSO
- `{{PUNTEGGIO_ELUSIONE}}` - Punteggio grezzo (0-12)
- `{{PERCENTILE_ELUSIONE}}` - Percentile (0-100)
- `{{RANGE_ELUSIONE}}` - ALTO/MEDIO/BASSO
- `{{PUNTEGGIO_ACCOMODAMENTO}}` - Punteggio grezzo (0-12)
- `{{PERCENTILE_ACCOMODAMENTO}}` - Percentile (0-100)
- `{{RANGE_ACCOMODAMENTO}}` - ALTO/MEDIO/BASSO

### Analisi Dettagliata
- `{{REFLECTION_INTRO_COMPETIZIONE}}` - Introduzione domande riflessione
- `{{REFLECTION_QUESTIONS_COMPETIZIONE}}` - Lista domande riflessione
- `{{REFLECTION_INTRO_COLLABORAZIONE}}` - Introduzione domande riflessione
- `{{REFLECTION_QUESTIONS_COLLABORAZIONE}}` - Lista domande riflessione
- (E così via per le altre modalità...)

### Risposte Complete
- `{{RISPOSTE_TABELLA}}` - Tabella con tutte le 30 risposte

## 🎨 LAYOUT E DESIGN

### Formato Pagina
- **Dimensioni:** A4 (210 x 297 mm)
- **Margini:** 2.5 cm tutti i lati
- **Orientamento:** Verticale

### Font e Stili
- **Font principale:** Arial, 11pt
- **Font titoli:** Arial Bold, 14-18pt
- **Interlinea:** 1.15
- **Colori:** Palette professionale

### Elementi Grafici
- **Barre percentili:** Colori corrispondenti alle modalità
- **Badge punteggi:** Rettangoli colorati con testo bianco
- **Box informativi:** Sfondo grigio chiaro, bordi colorati
- **Tabelle:** Righe alternate, header colorato

## 🔄 PROSSIMI PASSI

1. ✅ **Importa** il template in Google Docs
2. ✅ **Personalizza** logo e colori aziendali
3. 🔄 **Sviluppa** script per popolamento automatico
4. 🔄 **Integra** con dashboard TKI
5. 🔄 **Testa** generazione PDF
6. 🔄 **Ottimizza** layout e design

## 📝 NOTE TECNICHE

- Il template è **responsive** e si adatta a diverse dimensioni
- I **page break** sono configurati per stampa professionale
- I **colori** sono ottimizzati per stampa e visualizzazione digitale
- Il **layout** è progettato per massima leggibilità

## 🆘 SUPPORTO

Se hai problemi con l'importazione o la personalizzazione:
1. Verifica che il file HTML sia completo
2. Controlla che Google Docs supporti tutti gli stili CSS
3. Testa la stampa PDF prima di procedere
4. Contatta per assistenza tecnica
