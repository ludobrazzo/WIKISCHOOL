# WikiSchool - Guida Completa

## 📚 Panoramica del Progetto

**WikiSchool** è una piattaforma web moderna e responsiva pensata per caricare, organizzare, consultare e condividere appunti scolastici. Il sito permette agli utenti registrati di pubblicare materiali didattici, interagire con gli appunti della comunità, ricevere notifiche e usare un chatbot AI integrato (WikiBot) come supporto allo studio.

La piattaforma è progettata per:
- Studenti che vogliono digitalizzare i propri appunti
- Insegnanti che desiderano condividere materiale didattico
- Comunità di studenti che collaborano scambiandosi risorse

---

## ✨ Funzionalità Principali

### 1. **Autenticazione e Gestione Utenti**

#### Sistemi di Login
- **Login con Email e Password**: Sistema classico con validazione email
- **Google OAuth**: Login rapido tramite account Google
- **Registrazione**: Creazione nuovo account con nome, email e password
- **Password**: Minimo 6 caratteri (validazione lato Firebase)

#### Gestione dello Stato Utente
- Visualizzazione del nome utente loggato nella barra di navigazione
- Badge con colore differenziato quando loggato (verde #10b981)
- Pulsante di accesso al profilo personale visibile solo se loggati
- Pannello di logout posizionato nella sezione profilo
- Sessione persistente tramite Firebase Authentication
- Gestione automatica della visibilità di elementi basata sullo stato di login

#### Funzionalità Riservate
- Caricamento appunti (solo utenti autenticati)
- Messa di like (accesso negato con messaggio toast)
- Aggiunta ai preferiti (accesso negato con messaggio toast)
- Pubblicazione di commenti
- Visualizzazione notifiche
- Accesso al profilo personale

---

### 2. **Caricamento e Pubblicazione Appunti**

#### Modulo di Upload
Gli utenti loggati possono pubblicare appunti compilando i seguenti campi:

- **Titolo dell'appunto**: Campo di testo libero
- **Anno scolastico**: Selezione da dropdown
  - 1° Anno
  - 2° Anno
  - 3° Anno
  - 4° Anno
  - 5° Anno
- **Materia**: Selezione da 20 materie disponibili
  - Italiano
  - Latino
  - Storia
  - Filosofia
  - Educazione Civica
  - Matematica
  - Fisica
  - Scienze Naturali
  - Informatica
  - Disegno e Storia dell'Arte
  - Inglese
  - Francese
  - Spagnolo
  - Scienze Umane
  - Diritto ed Economia
  - Scienze Motorie
  - (E altre)

#### Metodi di Caricamento

**Opzione 1: Caricamento File Locale**
- Supporta: PDF, DOCX, JPG, PNG
- Caricamento tramite Cloudinary Upload Widget
- Salvataggio URL del file in Firestore
- Visualizzazione del nome file selezionato

**Opzione 2: Link Esterno**
- Copia/incolla di URL esterni
- Supporta: Google Drive, Canva, Docs, fogli Excel, presentazioni
- Ideale per contenuti condivisi online
- Memorizzazione del link in Firestore

#### Limitazioni
- Login obbligatorio (banner di avviso se non loggati)
- Validazione: almeno titolo, anno e materia
- Un file O un link (non entrambi)
- Dimensioni file limitate dalla policy di Cloudinary

#### Salvataggio Dati
- Memorizzazione in Firestore collection "projects"
- Campi salvati:
  - `title`: Titolo appunto
  - `year`: Anno scolastico
  - `subject`: Materia
  - `fileUrl`: URL del file caricato
  - `linkUrl`: URL esterno (se fornito)
  - `authorUid`: ID univoco dell'autore
  - `authorName`: Nome dell'autore
  - `createdAt`: Data e ora creazione
  - `likes`: Numero totali mi piace
  - `comments`: Array di commenti
  - `likedBy`: Set di UID che hanno messo like

---

### 3. **Archivio Appunti**

#### Pagina Dedicate (archivio.html)
Spazio per consultare, ricercare e interagire con gli appunti della comunità.

#### Visualizzazione Appunti

**Card Appunto** contiene:
- Icona/badge della materia
- Titolo dell'appunto
- Nome autore
- Data pubblicazione
- Anno scolastico
- Pulsanti di azione (vedi sotto)

#### Sistema di Ricerca e Filtri

**Ricerca Testuale**
- Filtro per parola chiave nel titolo
- Ricerca in tempo reale
- Case-insensitive

**Filtri Categoria**
- Filtro per materia (16 opzioni)
- Filtro per anno scolastico (1°-5° anno)
- Filtri cumulabili
- Reset filtri disponibile

#### Interazioni con Appunti

**Apertura/Visualizzazione**
- Link "Apri" per file caricati
- Apertura in nuova scheda
- Visualizzazione direttamente o download a discrezione del browser

**Like (Mi Piace)**
- Pulsante cuore per aggiungere mi piace
- Contatore numero like
- Utenti non loggati: richiesta login via toast
- Impedimento di mi piace duplicati

**Preferiti**
- Pulsante stella per salvare nei preferiti
- Memorizzazione in localStorage
- Accesso rapido dai preferiti
- Utenti non loggati: richiesta login via toast

**Commenti**
- Campo input per aggiungere commento
- Pulsante invio commento
- Visualizzazione lista commenti
- Nome autore commento
- Data/ora commento
- Generazione automatica notifiche all'autore dell'appunto

#### Ordinamento
- Ordinamento per data (più recenti prima)
- Ordinamento per numero like (più apprezzati)

---

### 4. **Sistema di Notifiche**

#### Campanella Notifiche
- Icona 🔔 visibile solo se loggati
- Posizionata nella barra superiore

#### Badge Notifiche
- Badge rosso con numero notifiche non lette
- Scompare quando tutte lette
- Aggiornamento in tempo reale

#### Pannello Notifiche
Finestra dropdown contenente:

**Intestazione**
- Titolo "Notifiche"
- Pulsante "Segna come lette"

**Lista Notifiche**
- Testo della notifica
- Data e ora dell'attività (formattate)
- Stato lettura (evidenziazione visiva)
- Scorrimento interno se numerose

**Azioni**
- Clic su notifica (se implementato): navigazione all'appunto
- "Segna come lette": marca tutte come lette
- Clic esterno al pannello: chiusura automatica

#### Trigger Notifiche
Vengono generate automaticamente per:
- **Nuovo commento su appunto**: L'autore riceve notifica quando qualcuno commenta
- **Nuovo like**: (opzionale, dipende dalla configurazione)
- **Commento su commento**: (se implementato)

#### Persistenza
- Salvataggio in Firestore collection "notifications"
- Memorizzazione ID notifiche lette in localStorage
- Recupero su refresh pagina

#### Formattazione Data
- Ora esatta (es. "14:30")
- Giorno stesso: "Oggi alle 14:30"
- Giorno precedente: "Ieri alle 14:30"
- Giorni passati: "5 giorni fa"

---

### 5. **Profilo Personale**

#### Sezione Profilo
Accessibile via pulsante "Il mio Profilo" in alto a destra.

#### Informazioni Utente
- Email dell'account
- Nome visualizzato
- Badge di identificazione

#### Gestione Contenuti Personali

**Elenco Appunti Pubblicati**
- Lista di tutti i file/appunti caricati dall'utente
- Visualizzazione titolo, materia, anno
- Informazioni di creazione

**Opzioni per Ogni Appunto**
- Pulsante "Visualizza": apertura dell'appunto
- Pulsante "Elimina": rimozione dell'appunto

#### Eliminazione Appunti

**Conferma Personalizzata**
- Finestra modale di conferma al posto del dialog del browser
- Messaggio: "Sei sicuro di voler eliminare questo appunto?"
- Pulsanti "Annulla" e "Conferma"
- Coerenza con lo stile grafico del sito
- Prevenzione eliminazioni accidentali

**Processo Eliminazione**
1. Clic su "Elimina"
2. Apertura modale conferma
3. Scelta utente (Annulla o Conferma)
4. Se confermato: eliminazione da Firestore
5. Rimozione file da Cloudinary (se presente)
6. Aggiornamento lista profilo
7. Toast di conferma eliminazione

#### Logout
- Pulsante "Logout" in alto nel profilo
- Disconnessione immediata
- Redirect alla home page
- Cancellazione sessione

---

### 6. **Toast di Avviso**

#### Sistema di Notifiche Non-Intrusive
Piccoli messaggi visivi per comunicare azioni o errori senza interrompere.

#### Posizionamento
- Parte inferiore della pagina
- Sopra la bolla del chatbot
- Non bloccanti (l'utente continua a navigare)

#### Esempi di Utilizzo
- "Accedi per mettere un mi piace"
- "Accedi per aggiungere ai preferiti"
- "Appunto eliminato con successo"
- "Commento pubblicato"
- "Errore: non è stato possibile salvare"
- "File caricato con successo"

#### Durata
- Visualizzazione automatica per 3-4 secondi
- Dismissione manuale possibile
- Animazione di fade out

---

### 7. **Chatbot AI - WikiBot**

#### Presentazione
WikiBot è un assistente AI integrato basato su Google Gemini, disponibile sia sulla homepage che nell'archivio.

#### Accesso
- **Pulsante Flottante**: Bolla arrotondata in basso a destra
- **Icona**: Freccia o chatbot (dipende dalla configurazione)
- **Visibilità**: Sempre presente quando non minimizzato

#### Apertura e Chiusura
- Clic sul pulsante flottante: apertura finestra
- Clic sul pulsante di chiusura (X): minimizzazione
- Clic fuori dalla finestra: minimizzazione automatica
- Riapertura: conserva la cronologia di chat

#### Invio Messaggi
- **Input testuale**: Campo testo a una o più righe
- **Pulsante invia**: Freccia o tasto Enter
- **Supporto messaggi lunghi**: Gestione automatica di testi estesi
- **Area di composizione**: Aggiornamento dinamico dell'altezza

#### Supporto Multimediale
- **Immagini/Foto**: Upload di immagini
  - Formati: JPG, PNG, WebP, GIF
  - Visualizzazione in chat
  - Analisi tramite Gemini Vision
- **Documenti**: (se supportato) Upload file
- **Barra di avanzamento**: Indicatore durante caricamento

#### Memoria Conversazione
- **Cronologia Sessione**: Mantenimento di tutti i messaggi durante la sessione
- **Contesto**: Gemini utilizza tutta la conversazione per risposte coerenti
- **Reset**: Chiudere il browser cancella la cronologia
- **Persistenza**: (opzionale) Possibile salvataggio in localStorage

#### Formattazione Risposte

**Supporto Markdown**
- **Grassetto**: `**testo**` → **testo**
- **Corsivo**: `*testo*` → *testo*
- **Titoli**: `# Titolo` → Titolo in H1
- **Titoli secondari**: `## Titolo` → Titolo in H2

**Liste Formattate**
- **Liste puntate**: `- elemento` → Bullet point
- **Liste numerate**: `1. elemento` → Elenco numerato
- **Lista nidificata**: Indentazione automatica

**Blocchi di Codice**
- **Codice inline**: `` `codice` ``
- **Blocchi codice**: 
  ```
  ```linguaggio
  codice
  ```
  ```
- **Colorazione sintattica**: Evidenziazione per linguaggi comuni
- **Scrolling**: Blocchi lunghi scrollabili senza uscire dalla chat

**Formule Matematiche**
- **Formule inline**: `$formula$` (rendering con MathJax)
- **Formule display**: `$$formula$$`
- **Simboli LaTeX**: Supporto completo
- **Formule lunghe**: Gestione ottimizzata per layout responsive

**Testi Lunghi**
- **Troncamento intelligente**: Mostra primi 500 caratteri + "Leggi di più"
- **Espansione**: Clic per visualizzare testo completo
- **Scroll interno**: Se comunque lungo

#### Modello AI Utilizzato
- **Nome**: Gemini 2.5 Flash
- **Versione**: gemini-2.5-flash (configurabile)
- **Proprietario**: Google

#### Implementazione Firebase AI Logic
- **Tecnologia**: Firebase AI Logic
- **Vantaggio**: API key Gemini non esposta nel frontend
- **Sicurezza**: Gestione token tramite backend Firebase
- **Configurazione**: Project ID e credenziali Firebase

#### Feedback e Limitazioni
- **Timeout**: Richieste cancellate dopo 30 secondi (configurabile)
- **Limite token**: Risposte limitate a 1024 token per coerenza
- **Errori rete**: Toast di errore se richiesta fallisce
- **Disclaimer**: Messaggio iniziale che avvisa sull'uso a scopo educativo

#### Area di Visualizzazione Chat
- **Sfondo**: Bianco/grigio con messaggi alternati per utente/AI
- **Messaggi Utente**: Stilati a destra (blu)
- **Messaggi Bot**: Stilati a sinistra (grigio)
- **Scroll automatico**: Ultimi messaggi sempre visibili
- **Indicatore di digitazione**: Puntini animati mentre il bot risponde

---

### 8. **Layout Responsive**

#### Design Mobile-First
Il sito è completamente ottimizzato per:
- **Computer**: Display full-width
- **Tablet**: Adattamento layout a 600-900px
- **Smartphone**: Adattamento layout <600px

#### Breakpoint CSS
- **Mobile**: < 600px
- **Tablet**: 600px - 1024px
- **Desktop**: > 1024px

#### Elementi Responsive

**Barra Superiore (Navbar)**
- Desktop: Layout orizzontale con tutti gli elementi
- Tablet: Compattazione dei pulsanti
- Mobile: Menu hamburger o ridimensionamento elementi

**Pannello Notifiche**
- Desktop: Dropdown posizionato a destra
- Mobile: Espanso a schermo intero o popup sovrapposto
- Altezza adattata per non uscire dallo schermo

**Chatbot**
- Desktop: Finestra 400x500px posizionata basso-destra
- Tablet: Adattamento dimensioni 350x450px
- Mobile: Finestra fullscreen o quasi
- Bolla flottante sempre accessibile

**Moduli di Caricamento**
- Desktop: Card con larghezza massima 600px
- Tablet: Ridimensionamento a 400px
- Mobile: Full-width con padding

**Pulsanti**
- Dimensioni aumentate su mobile (min 44x44px)
- Spacing tra pulsanti per evitare errori di tap
- Font size aumentato per leggibilità

**Card Appunti**
- Desktop: Griglia 3-4 colonne
- Tablet: Griglia 2 colonne
- Mobile: Single column (full-width)

**Profilo Utente**
- Desktop: Layout a colonne
- Mobile: Layout verticale (stack)
- Form inputs full-width su mobile

---

### 9. **Progressive Web App (PWA)**

#### manifest.json

**Metadati Applicazione**
```json
{
  "name": "WikiSchool",
  "short_name": "WikiSchool",
  "description": "Digitalizza e condividi i tuoi appunti scolastici"
}
```

**Icone App**
- Icona 192x192px: Per schermo principale dispositivi
- Icona 512x512px: Per splash screen e store
- Formato: PNG
- Colori: Coerenti con brand WikiSchool

**Tema e Colori**
- `theme_color`: #6366f1 (Indigo - colore primario)
- `background_color`: #f8fafc (Bianco/grigio chiaro)

**Comportamento**
- `display`: "standalone" (Sembra app nativa, non browser)
- `start_url`: "./index.html" (Pagina di avvio)

#### Service Worker (sw.js)

**Funzionalità**
- Registrazione automatica al caricamento
- Cache di file statici
- Fallback offline (se implementato)
- Gestione notifiche push (se implementato)

**Benefici**
- App installabile su desktop e mobile
- Caricamento più veloce se online
- Funzionalità offline ridotta ma funzionante
- Icona nel taskbar/home screen

#### Installazione
- **Desktop**: Menù browser → "Installa app"
- **Mobile iOS**: Share → Add to Home Screen
- **Mobile Android**: Menu browser → "Installa app"

---

## 🛠️ Struttura Tecnica

### Architettura Generale

```
WIKISCHOOL-main/
│
├── index.html                 # Home page / Dashboard
├── archivio.html              # Pagina archivio appunti
│
├── script.js                  # Logica JavaScript homepage
├── script_archivio.js         # Logica JavaScript archivio
│
├── styles.css                 # Stylesheet principale
│
├── manifest.json              # Configurazione PWA
├── sw.js                      # Service Worker
│
├── package.json               # Dipendenze e script npm
├── README.md                  # Documentazione
├── LICENSE                    # Licenza MIT
│
└── icons/
    ├── icon-192x192.png       # Icona app piccola
    └── icon-512x512.png       # Icona app grande
```

---

## 📄 Analisi Dettagliata dei File

### `index.html` (32 KB)

**Sezioni Principali**

#### Intestazione (Head)
```html
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>WikiSchool | Dashboard</title>
  <link rel="manifest" href="manifest.json" />
  <meta name="theme-color" content="#6366f1" />
  <link rel="apple-touch-icon" href="icons/icon-192x192.png">
  <link rel="stylesheet" href="styles.css" />
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.4.120/pdf.min.js"></script>
  <script src="https://upload-widget.cloudinary.com/global/all.js"></script>
  <script type="module" src="script.js"></script>
</head>
```

**Librerie Caricate**
- **PDF.js v3.4.120**: Per visualizzare preview PDF
- **Cloudinary Upload Widget**: Per upload file senza gestire backend
- **Firebase**: Tramite moduli ES6 in script.js

#### Sezione 1: Navbar (Navigazione)
```html
<nav class="glass-nav">
  <a href="index.html" class="brand">Wiki<span>School</span></a>
  
  <div class="nav-actions">
    <a href="archivio.html" class="nav-link">Esplora Archivio</a>
    <!-- Campanella Notifiche -->
    <div id="notification-wrapper">...</div>
    <!-- Pulsante Profilo -->
    <button id="profile-btn" class="btn-secondary">Il mio Profilo</button>
    <!-- Pulsante Login/Ciao User -->
    <button id="auth-btn" class="btn-primary">Login</button>
  </div>
</nav>
```

**Componenti**
- Logo "WikiSchool" linkato a home
- Link "Esplora Archivio" per navigazione
- Campanella notifiche (nascosta se non loggati)
- Pulsante profilo (nascosto se non loggati)
- Pulsante login dinamico (cambia testo se loggati)

#### Sezione 2: Hero e Form Upload
```html
<main class="hero">
  <h1>Digitalizza i tuoi <span>Appunti</span>.</h1>
  <p>Carica i tuoi appunti, crea riassunti e condividili...</p>
  
  <div class="upload-section card">
    <h2>📤 Carica un Appunto</h2>
    <p id="login-warning">Devi effettuare il login...</p>
    
    <div id="upload-form-container">
      <!-- Form visibile solo se loggati -->
      <input type="text" id="project-title" placeholder="Titolo..."/>
      <select id="project-year"><!-- Anni --></select>
      <select id="project-subject"><!-- Materie --></select>
      <input type="file" id="file-input" />
      <input type="url" id="doc-link" placeholder="Link esterno..."/>
      <button id="publish-btn">Pubblica Appunto</button>
    </div>
  </div>
</main>
```

**Logica**
- Avviso di login se non autenticati
- Form nascosto e disabilitato se non loggati
- Campo file: supporta PDF, DOCX, JPG, PNG
- Campo link: per URL esterni
- Validazione lato client e Firebase

#### Sezione 3: Modal Autenticazione
```html
<div id="auth-modal" class="modal-overlay">
  <div class="modal-content">
    <!-- Login Form -->
    <div id="auth-view-login">
      <input type="email" id="login-email" />
      <input type="password" id="login-password" />
      <button id="do-login">Accedi</button>
      <span id="go-to-reg">Registrati</span>
    </div>
    
    <!-- Registrazione Form -->
    <div id="auth-view-register" style="display: none;">
      <input type="text" id="reg-name" />
      <input type="email" id="reg-email" />
      <input type="password" id="reg-password" />
      <button id="do-register">Registrati</button>
      <span id="go-to-login">Accedi</span>
    </div>
    
    <!-- Login Google -->
    <button id="google-login">Continua con Google</button>
  </div>
</div>
```

**Funzionamento**
- Toggle tra login e registrazione
- Email e password con validazione Firebase
- Google OAuth integrato
- Modal overlay scuro per focus

#### Sezione 4: Modal Profilo
```html
<div id="profile-modal" class="modal-overlay">
  <div class="modal-content">
    <div id="profile-header">
      <p id="profile-email"></p>
      <button id="logout-btn">Logout</button>
    </div>
    
    <div id="profile-projects">
      <!-- Lista appunti utente -->
    </div>
  </div>
</div>
```

**Contenuto**
- Email utente loggato
- Lista di tutti gli appunti caricati
- Pulsanti "Visualizza" e "Elimina" per ogni appunto
- Pulsante logout

#### Sezione 5: Modal Conferma Eliminazione
```html
<div id="delete-modal" class="modal-overlay">
  <div class="modal-content">
    <p>Sei sicuro di voler eliminare questo appunto?</p>
    <button id="cancel-delete">Annulla</button>
    <button id="confirm-delete" class="btn-danger">Elimina</button>
  </div>
</div>
```

#### Sezione 6: Chatbot WikiBot
```html
<div id="chatbot-container">
  <div id="chatbot-toggle" class="chatbot-btn">💬</div>
  
  <div id="chatbot-window" class="chatbot-window">
    <div class="chatbot-header">
      <h3>WikiBot</h3>
      <button id="chatbot-close">&times;</button>
    </div>
    
    <div id="chatbot-messages" class="chatbot-messages"></div>
    
    <div class="chatbot-input-area">
      <input type="file" id="chatbot-image" accept="image/*" />
      <input type="text" id="chatbot-input" placeholder="Scrivi un messaggio..."/>
      <button id="chatbot-send">Invia</button>
    </div>
  </div>
</div>
```

---

### `archivio.html` (24 KB)

**Struttura Simile a index.html**

#### Sezioni Specifiche

**Barra di Ricerca e Filtri**
```html
<div class="search-filters">
  <input type="text" id="search-input" placeholder="Cerca appunti..."/>
  
  <select id="filter-subject">
    <option value="">Tutte le materie</option>
    <option value="Italiano">Italiano</option>
    <!-- ... -->
  </select>
  
  <select id="filter-year">
    <option value="">Tutti gli anni</option>
    <option value="1">1° Anno</option>
    <!-- ... -->
  </select>
  
  <button id="reset-filters">Reset</button>
</div>
```

**Grid di Appunti**
```html
<div id="projects-container" class="projects-grid">
  <!-- Card generati dinamicamente -->
  <div class="project-card">
    <h3>Titolo Appunto</h3>
    <p>Autore • Materia • Anno</p>
    <p>Data pubblicazione</p>
    
    <div class="project-actions">
      <a href="link-file" target="_blank">Apri</a>
      <button class="like-btn">❤️ 42</button>
      <button class="favorite-btn">⭐</button>
      <button class="comment-btn">💬</button>
    </div>
  </div>
</div>
```

#### Sezione Commenti
```html
<div class="comments-section">
  <h4>Commenti</h4>
  <input type="text" id="comment-input" placeholder="Scrivi un commento..."/>
  <button id="submit-comment">Pubblica</button>
  
  <div id="comments-list">
    <!-- Commenti generati dinamicamente -->
    <div class="comment">
      <p><strong>Nome Autore</strong> - Data</p>
      <p>Testo commento</p>
    </div>
  </div>
</div>
```

---

### `styles.css` (845 linee, 20 KB)

**Struttura**

#### 1. Variabili CSS (Tema)
```css
:root {
  --primary: #6366f1;        /* Indigo - colore principale */
  --primary-dark: #4f46e5;   /* Indigo scuro - hover */
  --secondary: #10b981;      /* Verde - successo */
  --danger: #ef4444;         /* Rosso - errori/delete */
  --warning: #f59e0b;        /* Arancio - avvisi */
  --text-primary: #1f2937;   /* Grigio scuro - testo */
  --text-muted: #6b7280;     /* Grigio medio - testo secondario */
  --bg-light: #f8fafc;       /* Bianco freddo - sfondo */
  --bg-card: #ffffff;        /* Bianco - card */
  --border: #e5e7eb;         /* Grigio chiaro - border */
  --shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  --radius: 8px;
}
```

#### 2. Reset e Base
```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  background: var(--bg-light);
  color: var(--text-primary);
}
```

#### 3. Navbar
```css
.glass-nav {
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid var(--border);
  display: flex;
  align-items: center;
  padding: 15px 30px;
  gap: 40px;
  position: sticky;
  top: 0;
  z-index: 100;
}

.brand {
  font-size: 1.5rem;
  font-weight: bold;
  color: var(--primary);
  text-decoration: none;
}

.brand span {
  color: var(--secondary);
}

.nav-actions {
  display: flex;
  gap: 20px;
  margin-left: auto;
  align-items: center;
}

/* Media query mobile */
@media (max-width: 768px) {
  .glass-nav {
    padding: 10px 15px;
    gap: 10px;
  }
  
  .nav-link {
    display: none; /* Nascosto su mobile */
  }
}
```

#### 4. Pulsanti
```css
.btn-primary {
  background: var(--primary);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: var(--radius);
  cursor: pointer;
  font-weight: 500;
  transition: background 0.3s;
}

.btn-primary:hover {
  background: var(--primary-dark);
}

.btn-primary:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn-secondary {
  background: var(--bg-light);
  color: var(--primary);
  border: 1px solid var(--border);
  padding: 10px 20px;
  border-radius: var(--radius);
  cursor: pointer;
  transition: all 0.3s;
}

.btn-danger {
  background: var(--danger);
  color: white;
}
```

#### 5. Card
```css
.card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 20px;
  box-shadow: var(--shadow);
  transition: transform 0.3s, box-shadow 0.3s;
}

.card:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15);
}
```

#### 6. Form
```css
.form-control {
  width: 100%;
  padding: 12px 15px;
  border: 1px solid var(--border);
  border-radius: var(--radius);
  font-size: 1rem;
  font-family: inherit;
  transition: border-color 0.3s, box-shadow 0.3s;
}

.form-control:focus {
  outline: none;
  border-color: var(--primary);
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
}
```

#### 7. Modal
```css
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 200;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s;
}

.modal-overlay.open {
  opacity: 1;
  visibility: visible;
}

.modal-content {
  background: var(--bg-card);
  border-radius: var(--radius);
  padding: 30px;
  max-width: 500px;
  width: 90%;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
  position: relative;
}

.close-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
  color: var(--text-muted);
}
```

#### 8. Notifiche (Toast)
```css
.toast {
  position: fixed;
  bottom: 100px; /* Sopra chatbot */
  left: 20px;
  background: var(--text-primary);
  color: white;
  padding: 15px 20px;
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  animation: slideUp 0.3s ease;
  z-index: 150;
}

@keyframes slideUp {
  from {
    transform: translateY(20px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

.toast.success {
  background: var(--secondary);
}

.toast.error {
  background: var(--danger);
}

.toast.warning {
  background: var(--warning);
  color: var(--text-primary);
}
```

#### 9. Pannello Notifiche
```css
.notification-wrapper {
  position: relative;
}

.notification-btn {
  position: relative;
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
}

.notification-badge {
  position: absolute;
  top: -8px;
  right: -8px;
  background: var(--danger);
  color: white;
  border-radius: 50%;
  min-width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.75rem;
  font-weight: bold;
}

.notification-panel {
  position: absolute;
  top: 100%;
  right: 0;
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  width: 300px;
  max-height: 400px;
  overflow-y: auto;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s;
  transform: translateY(-10px);
}

.notification-panel.open {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.notification-item {
  padding: 12px 15px;
  border-bottom: 1px solid var(--border);
  cursor: pointer;
  transition: background 0.2s;
}

.notification-item:hover {
  background: var(--bg-light);
}

.notification-item.unread {
  background: rgba(99, 102, 241, 0.05);
  border-left: 3px solid var(--primary);
}
```

#### 10. Chatbot
```css
.chatbot-container {
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 140;
}

.chatbot-btn {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: var(--primary);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  cursor: pointer;
  box-shadow: var(--shadow);
  transition: all 0.3s;
}

.chatbot-btn:hover {
  transform: scale(1.1);
  box-shadow: 0 10px 20px rgba(99, 102, 241, 0.3);
}

.chatbot-window {
  position: absolute;
  bottom: 80px;
  right: 0;
  width: 400px;
  height: 500px;
  background: var(--bg-card);
  border-radius: var(--radius);
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s;
  transform: scale(0.9);
}

.chatbot-window.open {
  opacity: 1;
  visibility: visible;
  transform: scale(1);
}

.chatbot-header {
  background: var(--primary);
  color: white;
  padding: 15px;
  border-radius: var(--radius) var(--radius) 0 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.chatbot-messages {
  flex: 1;
  overflow-y: auto;
  padding: 15px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.chatbot-message {
  max-width: 80%;
  padding: 10px 15px;
  border-radius: var(--radius);
  word-wrap: break-word;
}

.chatbot-message.user {
  align-self: flex-end;
  background: var(--primary);
  color: white;
}

.chatbot-message.bot {
  align-self: flex-start;
  background: var(--bg-light);
  color: var(--text-primary);
}

.chatbot-input-area {
  display: flex;
  gap: 10px;
  padding: 15px;
  border-top: 1px solid var(--border);
}

.chatbot-input-area input {
  flex: 1;
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 10px;
}

/* Mobile */
@media (max-width: 768px) {
  .chatbot-window {
    width: calc(100vw - 20px);
    height: 70vh;
  }
}
```

#### 11. Layout Grid
```css
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  padding: 30px;
}

@media (max-width: 1024px) {
  .projects-grid {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  }
}

@media (max-width: 768px) {
  .projects-grid {
    grid-template-columns: 1fr;
  }
}
```

---

### `script.js` (556 linee)

#### Sezione 1: Inizializzazione Firebase
```javascript
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
import {
  getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword,
  signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut, updateProfile
} from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
import {
  getFirestore, collection, addDoc, query, where, getDocs, deleteDoc, doc, onSnapshot
} from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

const firebaseConfig = {
  apiKey: "AIzaSyDl9CCciK9P1od4ITzpskYsP5Sa5N7ukOE",
  authDomain: "wikischool-vero.firebaseapp.com",
  projectId: "wikischool-vero",
  storageBucket: "wikischool-vero.firebasestorage.app",
  messagingSenderId: "373765015160",
  appId: "1:373765015160:web:a709c39d0a3529cc04cf8d",
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const provider = new GoogleAuthProvider();
```

**Componenti Firebase**
- **Firebase App**: Istanza principale
- **Authentication**: Per login/registrazione
- **Firestore**: Database dei documenti
- **Google Provider**: OAuth con Google

#### Sezione 2: Configurazione Cloudinary
```javascript
const CLOUD_NAME = "dkzbg6vyo";
const UPLOAD_PRESET = "unsigned_preset_123";
```

**Utilizzo**
- Upload file senza backend
- Unsigned preset: non richiede credenziali
- File salvati in Cloudinary CDN
- URL ritornato e salvato in Firestore

#### Sezione 3: Gestione Autenticazione
```javascript
let currentUser = null;

onAuthStateChanged(auth, (user) => {
  if (user) {
    currentUser = user;
    authBtn.innerText = "Ciao, " + (user.displayName || "Studente");
    authBtn.style.background = "#10b981";
    profileBtn.style.display = "block";
    notificationWrapper.style.display = "block";
    startNotifications(user);
    warningMsg.style.display = "none";
    formContainer.style.display = "block";
  } else {
    currentUser = null;
    authBtn.innerText = "Login";
    authBtn.style.background = "var(--primary)";
    profileBtn.style.display = "none";
    notificationWrapper.style.display = "none";
    stopNotifications();
    warningMsg.style.display = "block";
    formContainer.style.display = "none";
  }
});
```

**Logica**
- Monitora lo stato di autenticazione in tempo reale
- Se loggato: mostra profilo, notifiche e form upload
- Se non loggato: nasconde elementi e mostra avviso
- Aggiornamento UI automatico

#### Sezione 4: Campanella Notifiche
```javascript
function startNotifications(user) {
  const q = query(collection(db, "projects"), where("authorUid", "==", user.uid));
  
  unsubscribeNotifications = onSnapshot(q, (snapshot) => {
    currentNotifications = [];
    
    snapshot.forEach((doc) => {
      const project = doc.data();
      if (project.comments) {
        project.comments.forEach((comment) => {
          currentNotifications.push({
            id: doc.id + "_" + comment.timestamp,
            text: comment.user + " ha commentato: " + comment.text,
            date: comment.timestamp
          });
        });
      }
    });
    
    renderNotifications();
  });
}

function renderNotifications(uid = currentUser?.uid) {
  const unreadCount = currentNotifications.filter((n) => !readNotificationIds.has(n.id)).length;
  
  if (unreadCount > 0) {
    notificationBadge.innerText = unreadCount;
    notificationBadge.style.display = "inline-block";
  } else {
    notificationBadge.style.display = "none";
  }
  
  notificationList.innerHTML = currentNotifications.map((n) => {
    const isUnread = !readNotificationIds.has(n.id);
    return `
      <div class="notification-item ${isUnread ? "unread" : ""}">
        <p>${escapeHtml(n.text)}</p>
        <small>${formatNotificationDate(n.date)}</small>
      </div>
    `;
  }).join("");
}
```

**Funzionamento**
- Query Firestore per appunti dell'utente
- Listener real-time su modifiche
- Estrae commenti e crea notifiche
- Mostra badge con numero non lette
- Formatta data in modo leggibile

#### Sezione 5: Caricamento Appunti
```javascript
const publishBtn = document.getElementById("publish-btn");

publishBtn?.addEventListener("click", async () => {
  const title = document.getElementById("project-title").value;
  const year = document.getElementById("project-year").value;
  const subject = document.getElementById("project-subject").value;
  const docLink = document.getElementById("doc-link").value;
  
  if (!title || !year || !subject) {
    showToast("Compila tutti i campi", "error");
    return;
  }
  
  publishBtn.disabled = true;
  
  try {
    let fileUrl = "";
    
    if (fileInput.files[0]) {
      fileUrl = await uploadFileToCloudinary(fileInput.files[0]);
    }
    
    await addDoc(collection(db, "projects"), {
      title,
      year,
      subject,
      fileUrl,
      linkUrl: docLink,
      authorUid: currentUser.uid,
      authorName: currentUser.displayName || "Anonimo",
      createdAt: new Date(),
      likes: 0,
      comments: []
    });
    
    showToast("Appunto pubblicato!", "success");
    document.getElementById("project-title").value = "";
    // Reset form...
  } catch (err) {
    showToast("Errore: " + err.message, "error");
  } finally {
    publishBtn.disabled = false;
  }
});
```

**Flusso**
1. Validazione campi
2. Upload file a Cloudinary (se presente)
3. Salvataggio documento in Firestore
4. Reset form
5. Feedback utente via toast

#### Sezione 6: Upload File
```javascript
async function uploadFileToCloudinary(file) {
  const formData = new FormData();
  formData.append("file", file);
  formData.append("upload_preset", UPLOAD_PRESET);
  formData.append("cloud_name", CLOUD_NAME);
  
  const response = await fetch(`https://api.cloudinary.com/v1_1/${CLOUD_NAME}/auto/upload`, {
    method: "POST",
    body: formData
  });
  
  const data = await response.json();
  return data.secure_url;
}
```

**Processo**
- FormData con file e preset
- POST a Cloudinary API
- Ritorna URL file salvato
- URL salvato in Firestore

#### Sezione 7: Autenticazione
```javascript
document.getElementById("do-login")?.addEventListener("click", async () => {
  const email = document.getElementById("login-email").value;
  const password = document.getElementById("login-password").value;
  
  try {
    await signInWithEmailAndPassword(auth, email, password);
    modal.classList.remove("open");
    showToast("Login effettuato!", "success");
  } catch (err) {
    showToast("Errore login: " + err.message, "error");
  }
});

document.getElementById("do-register")?.addEventListener("click", async () => {
  const name = document.getElementById("reg-name").value;
  const email = document.getElementById("reg-email").value;
  const password = document.getElementById("reg-password").value;
  
  try {
    const userCred = await createUserWithEmailAndPassword(auth, email, password);
    await updateProfile(userCred.user, { displayName: name });
    modal.classList.remove("open");
    showToast("Registrazione completata!", "success");
  } catch (err) {
    showToast("Errore registrazione: " + err.message, "error");
  }
});

document.getElementById("google-login")?.addEventListener("click", async () => {
  try {
    await signInWithPopup(auth, provider);
    modal.classList.remove("open");
    showToast("Login Google effettuato!", "success");
  } catch (err) {
    showToast("Errore Google login: " + err.message, "error");
  }
});
```

#### Sezione 8: Profilo Utente
```javascript
profileBtn?.addEventListener("click", () => {
  profileModal.classList.add("open");
  loadProfileProjects();
});

async function loadProfileProjects() {
  const q = query(collection(db, "projects"), where("authorUid", "==", currentUser.uid));
  const snapshot = await getDocs(q);
  
  const list = document.getElementById("profile-projects");
  list.innerHTML = snapshot.docs.map((doc) => {
    const data = doc.data();
    return `
      <div class="profile-project">
        <h4>${data.title}</h4>
        <p>${data.subject} • ${data.year}° anno</p>
        <button onclick="viewProject('${doc.id}')">Visualizza</button>
        <button onclick="deleteProjectPrompt('${doc.id}')">Elimina</button>
      </div>
    `;
  }).join("");
}
```

#### Sezione 9: Eliminazione Appunti
```javascript
function deleteProjectPrompt(projectId) {
  pendingDeleteId = projectId;
  deleteModal.classList.add("open");
}

confirmDeleteBtn?.addEventListener("click", async () => {
  if (pendingDeleteId) {
    try {
      await deleteDoc(doc(db, "projects", pendingDeleteId));
      deleteModal.classList.remove("open");
      showToast("Appunto eliminato", "success");
      loadProfileProjects();
    } catch (err) {
      showToast("Errore eliminazione: " + err.message, "error");
    }
  }
});
```

#### Sezione 10: Chatbot WikiBot

**Inizializzazione**
```javascript
const chatbotToggle = document.getElementById("chatbot-toggle");
const chatbotWindow = document.getElementById("chatbot-window");
const chatbotInput = document.getElementById("chatbot-input");
const chatbotSend = document.getElementById("chatbot-send");
const chatbotMessages = document.getElementById("chatbot-messages");

chatbotToggle?.addEventListener("click", () => {
  chatbotWindow.classList.toggle("open");
});

chatbotSend?.addEventListener("click", () => {
  const message = chatbotInput.value.trim();
  if (message) {
    sendChatMessage(message);
    chatbotInput.value = "";
  }
});
```

**Invio Messaggi**
```javascript
let chatHistory = [];

async function sendChatMessage(message) {
  // Aggiungi messaggio utente
  addChatBubble(message, "user");
  
  // Aggiungi alle cronologia
  chatHistory.push({ role: "user", content: message });
  
  // Mostra indicatore digitazione
  showTypingIndicator();
  
  try {
    // Chiama API Gemini tramite Firebase
    const response = await fetch("https://firebaseml.googleapis.com/v1/projects/YOUR_PROJECT/instances:predict", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "Authorization": `Bearer ${await auth.currentUser?.getIdToken()}`
      },
      body: JSON.stringify({
        instances: [{ content_0: chatHistory }]
      })
    });
    
    const data = await response.json();
    const botMessage = data.predictions[0];
    
    chatHistory.push({ role: "model", content: botMessage });
    addChatBubble(botMessage, "bot");
    removeTypingIndicator();
  } catch (err) {
    showToast("Errore chatbot: " + err.message, "error");
    removeTypingIndicator();
  }
}

function addChatBubble(text, sender) {
  const div = document.createElement("div");
  div.className = `chatbot-message ${sender}`;
  div.innerHTML = formatMarkdown(text); // Supporto markdown
  chatbotMessages.appendChild(div);
  chatbotMessages.scrollTop = chatbotMessages.scrollHeight;
}
```

**Formattazione Markdown**
```javascript
function formatMarkdown(text) {
  // Grassetto: **testo**
  text = text.replace(/\*\*(.*?)\*\*/g, "<strong>$1</strong>");
  
  // Corsivo: *testo*
  text = text.replace(/\*(.*?)\*/g, "<em>$1</em>");
  
  // Codice: `codice`
  text = text.replace(/`(.*?)`/g, "<code>$1</code>");
  
  // Blocchi codice
  text = text.replace(/```(.*?)```/gs, "<pre><code>$1</code></pre>");
  
  // Titoli: # Titolo
  text = text.replace(/^# (.*?)$/gm, "<h1>$1</h1>");
  text = text.replace(/^## (.*?)$/gm, "<h2>$1</h2>");
  text = text.replace(/^### (.*?)$/gm, "<h3>$1</h3>");
  
  // Liste: - elemento
  text = text.replace(/^- (.*?)$/gm, "<li>$1</li>");
  text = text.replace(/(<li>.*?<\/li>)/s, "<ul>$1</ul>");
  
  return text;
}
```

**Supporto Immagini**
```javascript
const chatbotImageInput = document.getElementById("chatbot-image");

chatbotImageInput?.addEventListener("change", async (e) => {
  const file = e.target.files[0];
  if (file) {
    const reader = new FileReader();
    reader.onload = async (event) => {
      const base64 = event.target.result;
      // Invia immagine a Gemini Vision
      await sendChatWithImage(base64);
    };
    reader.readAsDataURL(file);
  }
});
```

#### Sezione 11: Funzioni Utility
```javascript
function showToast(message, type = "info") {
  const toast = document.createElement("div");
  toast.className = `toast ${type}`;
  toast.innerText = message;
  document.body.appendChild(toast);
  
  setTimeout(() => {
    toast.style.animation = "slideDown 0.3s ease";
    setTimeout(() => toast.remove(), 300);
  }, 3000);
}

function escapeHtml(text) {
  const map = {
    "&": "&amp;",
    "<": "&lt;",
    ">": "&gt;",
    '"': "&quot;",
    "'": "&#039;"
  };
  return text.replace(/[&<>"']/g, (m) => map[m]);
}
```

---

### `script_archivio.js` (391 linee)

#### Sezione 1: Caricamento Appunti
```javascript
let allProjects = [];

async function loadProjects() {
  const snapshot = await getDocs(collection(db, "projects"));
  allProjects = snapshot.docs.map((doc) => ({
    id: doc.id,
    ...doc.data()
  }));
  
  renderProjects(allProjects);
}

function renderProjects(projects) {
  const container = document.getElementById("projects-container");
  container.innerHTML = projects.map((proj) => `
    <div class="project-card">
      <div class="project-header">
        <h3>${escapeHtml(proj.title)}</h3>
        <span class="subject-badge">${proj.subject}</span>
      </div>
      
      <p class="project-meta">
        <strong>${proj.authorName}</strong> • ${proj.year}° anno
      </p>
      
      <p class="project-date">${formatDate(proj.createdAt.toDate())}</p>
      
      <div class="project-actions">
        ${proj.fileUrl ? `<a href="${proj.fileUrl}" target="_blank" class="btn-secondary">📄 Apri</a>` : ""}
        ${proj.linkUrl ? `<a href="${proj.linkUrl}" target="_blank" class="btn-secondary">🔗 Apri Link</a>` : ""}
        <button class="like-btn" onclick="toggleLike('${proj.id}')">❤️ ${proj.likes || 0}</button>
        <button class="favorite-btn" onclick="toggleFavorite('${proj.id}')">⭐</button>
      </div>
      
      <div class="comments-section">
        <h4>Commenti (${proj.comments?.length || 0})</h4>
        ${(proj.comments || []).map((c) => `
          <div class="comment">
            <strong>${c.user}</strong> - ${formatDate(c.timestamp.toDate())}
            <p>${escapeHtml(c.text)}</p>
          </div>
        `).join("")}
        
        <input type="text" class="comment-input" placeholder="Scrivi un commento..." data-project-id="${proj.id}"/>
        <button class="btn-primary" onclick="submitComment('${proj.id}')">Pubblica</button>
      </div>
    </div>
  `).join("");
}
```

#### Sezione 2: Ricerca e Filtri
```javascript
const searchInput = document.getElementById("search-input");
const filterSubject = document.getElementById("filter-subject");
const filterYear = document.getElementById("filter-year");

function filterAndSearch() {
  let filtered = allProjects;
  
  const searchTerm = searchInput.value.toLowerCase();
  if (searchTerm) {
    filtered = filtered.filter((p) =>
      p.title.toLowerCase().includes(searchTerm)
    );
  }
  
  const subject = filterSubject.value;
  if (subject) {
    filtered = filtered.filter((p) => p.subject === subject);
  }
  
  const year = filterYear.value;
  if (year) {
    filtered = filtered.filter((p) => p.year === year);
  }
  
  renderProjects(filtered);
}

searchInput?.addEventListener("input", filterAndSearch);
filterSubject?.addEventListener("change", filterAndSearch);
filterYear?.addEventListener("change", filterAndSearch);
```

#### Sezione 3: Like
```javascript
async function toggleLike(projectId) {
  if (!currentUser) {
    showToast("Devi accedere per mettere un mi piace", "warning");
    return;
  }
  
  const projectRef = doc(db, "projects", projectId);
  const projectSnap = await getDoc(projectRef);
  const project = projectSnap.data();
  
  const hasLiked = project.likedBy?.includes(currentUser.uid);
  
  if (hasLiked) {
    // Rimuovi like
    await updateDoc(projectRef, {
      likes: (project.likes || 1) - 1,
      likedBy: project.likedBy.filter((uid) => uid !== currentUser.uid)
    });
  } else {
    // Aggiungi like
    await updateDoc(projectRef, {
      likes: (project.likes || 0) + 1,
      likedBy: [...(project.likedBy || []), currentUser.uid]
    });
  }
  
  loadProjects();
}
```

#### Sezione 4: Preferiti (localStorage)
```javascript
function toggleFavorite(projectId) {
  let favorites = JSON.parse(localStorage.getItem("wikischool_favorites") || "[]");
  
  if (favorites.includes(projectId)) {
    favorites = favorites.filter((id) => id !== projectId);
  } else {
    favorites.push(projectId);
  }
  
  localStorage.setItem("wikischool_favorites", JSON.stringify(favorites));
  renderFavoritesUI(favorites);
}

function renderFavoritesUI(favorites) {
  document.querySelectorAll(".favorite-btn").forEach((btn) => {
    const projectId = btn.dataset.projectId;
    if (favorites.includes(projectId)) {
      btn.classList.add("active");
      btn.style.color = "var(--warning)";
    } else {
      btn.classList.remove("active");
      btn.style.color = "inherit";
    }
  });
}
```

#### Sezione 5: Commenti
```javascript
async function submitComment(projectId) {
  if (!currentUser) {
    showToast("Devi accedere per commentare", "warning");
    return;
  }
  
  const projectRef = doc(db, "projects", projectId);
  const projectSnap = await getDoc(projectRef);
  const project = projectSnap.data();
  
  const commentInput = document.querySelector(`input[data-project-id="${projectId}"]`);
  const comment = commentInput.value.trim();
  
  if (!comment) return;
  
  const newComment = {
    user: currentUser.displayName || "Anonimo",
    text: comment,
    timestamp: new Date(),
    userUid: currentUser.uid
  };
  
  await updateDoc(projectRef, {
    comments: [...(project.comments || []), newComment]
  });
  
  // Crea notifica per autore
  if (project.authorUid !== currentUser.uid) {
    await addDoc(collection(db, "notifications"), {
      recipientUid: project.authorUid,
      text: `${currentUser.displayName} ha commentato il tuo appunto "${project.title}"`,
      projectId: projectId,
      date: new Date(),
      read: false
    });
  }
  
  commentInput.value = "";
  loadProjects();
  showToast("Commento pubblicato!", "success");
}
```

---

### `manifest.json` (22 linee)

```json
{
  "name": "WikiSchool",
  "short_name": "WikiSchool",
  "description": "Digitalizza e condividi i tuoi appunti scolastici",
  "start_url": "./index.html",
  "display": "standalone",
  "background_color": "#f8fafc",
  "theme_color": "#6366f1",
  "icons": [
    {
      "src": "icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

**Utilizzo**
- Definisce metadati della PWA
- Icone per installation
- Colori tema
- URL di avvio

---

### `sw.js` (Service Worker)

```javascript
const CACHE_NAME = "wikischool-v1";
const urlsToCache = [
  "./",
  "./index.html",
  "./archivio.html",
  "./styles.css",
  "./script.js",
  "./script_archivio.js",
  "./manifest.json"
];

// Installa cache
self.addEventListener("install", (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME).then((cache) => {
      return cache.addAll(urlsToCache);
    })
  );
});

// Usa cache se offline
self.addEventListener("fetch", (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});

// Cleanup cache vecchi
self.addEventListener("activate", (event) => {
  event.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames.map((name) => {
          if (name !== CACHE_NAME) {
            return caches.delete(name);
          }
        })
      );
    })
  );
});
```

**Funzioni**
- Cache statico al primo caricamento
- Fallback offline
- Cleanup cache

---

## 🔧 Tecnologie Utilizzate

### Frontend
- **HTML5**: Struttura pagine
- **CSS3**: Styling responsive con variabili e flexbox/grid
- **JavaScript (ES6+)**: Logica applicazione con moduli

### Backend e Servizi
- **Firebase Authentication**: Login/registrazione utenti
  - Email/Password
  - Google OAuth
  - Gestione sessioni
  
- **Firebase Firestore**: Database NoSQL
  - Collection "projects": appunti
  - Collection "notifications": notifiche
  - Real-time listeners
  - Queries con where/getDocs
  
- **Firebase AI Logic**: Integrazione Gemini senza API key frontend
  - API sicura tramite credenziali Firebase
  - Modello: gemini-2.5-flash
  
- **Cloudinary**: Upload e hosting file
  - Unsigned upload (no credenziali)
  - URL file ritornato
  - CDN globale

### Librerie Esterne
- **PDF.js (3.4.120)**: Rendering PDF in browser
  - Visualizzazione pagine
  - Estrazione testo
  
- **Cloudinary Upload Widget**: Interfaccia upload file
  
- **MathJax**: Rendering formule matematiche
  - Supporto LaTeX
  - Rendering dinamico
  
- **Firebase SDK (10.7.1)**: Moduli Firebase
  - Auth module
  - Firestore module
  - Cloud Functions (se usate)

### Build e Deploy
- **npm**: Gestione dipendenze
  - Package: `serve@11.2.0`
  - Script: `npm start`
- **Serve**: Server locale development

### Altro
- **Local Storage**: Persistenza dati client
  - Notifiche lette
  - Preferiti
  - Preferenze utente

---

## 📊 Database Firestore

### Struttura Collection "projects"
```javascript
{
  id: "auto-generated-by-firestore",
  title: "Titolo Appunto",
  year: "4",
  subject: "Matematica",
  fileUrl: "https://cloudinary.com/...",  // Da Cloudinary
  linkUrl: "https://drive.google.com/...",  // Esterno
  authorUid: "uid-firebase",
  authorName: "Mario Rossi",
  createdAt: Timestamp,
  likes: 42,
  likedBy: ["uid1", "uid2", ...],  // Set di UID
  comments: [
    {
      user: "Luca Bianchi",
      text: "Utile!",
      timestamp: Timestamp,
      userUid: "uid3"
    },
    ...
  ]
}
```

### Struttura Collection "notifications"
```javascript
{
  id: "auto-generated",
  recipientUid: "uid-autore-appunto",
  text: "Descrizione notifica",
  projectId: "id-appunto",
  date: Timestamp,
  read: false
}
```

---

## 🎨 Variabili CSS Personalizzabili

```css
:root {
  --primary: #6366f1;          /* Indigo - Colore principale */
  --primary-dark: #4f46e5;     /* Hover stato primario */
  --secondary: #10b981;        /* Verde - Successo */
  --danger: #ef4444;           /* Rosso - Errori */
  --warning: #f59e0b;          /* Arancio - Avvisi */
  --text-primary: #1f2937;     /* Testo scuro */
  --text-muted: #6b7280;       /* Testo secondario */
  --bg-light: #f8fafc;         /* Sfondo chiaro */
  --bg-card: #ffffff;          /* Card bianco */
  --border: #e5e7eb;           /* Bordi grigio */
  --shadow: 0 4px 6px rgba(...);  /* Ombra */
  --radius: 8px;               /* Bordi arrotondati */
}
```

---

## 🚀 Come Iniziare

### Installazione Locale
```bash
npm install
npm start
```

### Deploy
- Deploy su Firebase Hosting
- Deploy su Netlify
- Deploy su Vercel
- Deploy su GitHub Pages

---

## 📝 Note di Sviluppo

### Points di Estensione
1. **Autenticazione**: Aggiungere 2FA, OAuth provider aggiuntivi
2. **Caricamento File**: Implementare drag-drop, preview file
3. **Notifiche**: Aggiungere notifiche push browser
4. **Chatbot**: Fine-tuning prompt Gemini, history persistente
5. **Social**: Like a commenti, follow utenti, messaggi privati
6. **Categorie**: Tag aggiuntivi, ricerca avanzata
7. **Analytics**: Tracking uso, statistiche appunti
8. **Monetizzazione**: Contenuti premium, pubblicità

---

## 📄 Licenza

MIT License - Vedi FILE LICENSE

---

## ✍️ Autore

Sviluppato come piattaforma collaborativa per studenti.

---

**Versione Documentazione**: 1.0
**Data Ultima Modifica**: Maggio 2026
**Stato Progetto**: Produzione
