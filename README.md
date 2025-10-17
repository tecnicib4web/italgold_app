# Italgold App Tablet

Sistema di gestione e tracking delle commesse, sviluppato da B4web s.r.l. per Italgold (ITG Jewellery Group). L'applicazione permette agli operatori di registrare in tempo reale le attività svolte, monitorare lo stato di avanzamento delle lavorazioni e migliorare il coordinamento delle risorse.

## Caratteristiche Principali

- **Autenticazione**: Login tramite username e password integrato con il gestionale esistente
- **Dashboard Operatore**: Visualizzazione commesse assegnate con tutte le informazioni rilevanti
- **Time Tracking**:
  - Avvio/stop tracking su singola commessa
  - Tracking parallelo su multiple commesse (fino a 10 contemporaneamente)
  - Timer live con visualizzazione tempo effettivo
  - Calcolo automatico tempo totale giornaliero
- **Gestione Commesse**:
  - Visualizzazione numero commessa, pezzi, fase di lavorazione
  - Descrizione dettagliata della lavorazione
  - Tempo target vs tempo effettivo
- **Visualizzazione Contenuti**:
  - Foto prodotto a schermo intero
  - Scheda tecnica del prodotto
- **Notifiche Automatiche**:
  - Promemoria pausa pranzo (12:30)
  - Promemoria fine turno
- **Conferme di Sicurezza**:
  - Conferma per avvio tracking multiplo
  - Conferma per cambio commessa durante tracking
  - Conferma logout

## Tecnologie Utilizzate

### Backend
- **Framework**: Laravel
- **Database**: MSSQL
- **API**: RESTful API
- **Autenticazione**: Laravel

### Frontend
- **Framework**: React
- **Gestione Stato**: React Context API / Redux
- **Routing**: React Router
- **UI**: Tailwind CSS / Material-UI
- **HTTP Client**: Axios
- **Timer Management**: Custom hooks per gestione cronometri

## Requisiti di Sistema

### Backend
- PHP >= 8.1
- Node.js >= 16.x (per compilazione asset)

### Frontend
- Node.js >= 16.x
- npm o yarn

## Installazione

### Backend (Laravel)

```bash
# Clona il repository
git clone [repository-url]
cd italgold

# Installa le dipendenze PHP
composer install

# Copia il file di configurazione
cp .env.example .env

# Genera la chiave dell'applicazione
php artisan key:generate

# Configura il database nel file .env
# DB_CONNECTION=mysql
# DB_HOST=127.0.0.1
# DB_PORT=3306
# DB_DATABASE=italgold
# DB_USERNAME=root
# DB_PASSWORD=

# Esegui le migrazioni
php artisan migrate

# Avvia il server di sviluppo
php artisan serve
```

### Frontend (React)

```bash
# Naviga nella cartella frontend
cd frontend

# Installa le dipendenze
npm install

# Copia il file di configurazione
cp .env.example .env

# Configura l'URL dell'API nel file .env
# REACT_APP_API_URL=http://localhost:8000/api

# Avvia il server di sviluppo
npm start
```

## Configurazione

### Variabili d'Ambiente Backend (.env)

```env
APP_NAME=Italgold
APP_ENV=local
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=italgold
DB_USERNAME=root
DB_PASSWORD=

# Orari notifiche
LUNCH_BREAK_TIME=12:30
END_SHIFT_TIME=17:30

# Limite tracking parallelo
MAX_PARALLEL_TRACKING=10
```

### Variabili d'Ambiente Frontend (.env)

```env
REACT_APP_API_URL=http://localhost:8000/api
REACT_APP_NAME=Italgold
```

## Struttura del Progetto
(qui la struttura del progetto)

struttura d'esempio:
```
italgold/
├── backend/                 # Applicazione Laravel
│   ├── app/
│   │   ├── Http/
│   │   │   ├── Controllers/
│   │   │   └── Middleware/
│   │   ├── Models/
│   │   └── Services/
│   ├── database/
│   │   ├── migrations/
│   │   └── seeders/
│   ├── routes/
│   │   └── api.php
│   └── ...
│
└── frontend/               # Applicazione React
    ├── public/
    ├── src/
    │   ├── components/
    │   │   ├── Dashboard/
    │   │   ├── Auth/
    │   │   ├── Modals/
    │   │   └── Common/
    │   ├── hooks/
    │   ├── services/
    │   ├── context/
    │   ├── utils/
    │   └── App.js
    └── ...
```

## API Endpoints

### Autenticazione
- `POST /api/login` - Login utente
- `POST /api/logout` - Logout utente
- `GET /api/user` - Informazioni utente autenticato

### Commesse
- `GET /api/commesse` - Lista commesse assegnate all'utente
- `GET /api/commesse/{id}` - Dettagli commessa
- `GET /api/commesse/{id}/scheda-tecnica` - Scheda tecnica commessa

### Tracking
- `POST /api/tracking/start` - Avvia tracking
- `POST /api/tracking/stop` - Ferma tracking
- `GET /api/tracking/active` - Tracking attivi
- `GET /api/tracking/daily-total` - Totale giornaliero

(CORREGGERE O AGGIUNGERE LE EFFETTIVE API UTILIZZATE)

## Funzionalità Chiave

### Time Tracking
Il sistema gestisce il tracking del tempo attraverso:
- Timer singolo per commessa
- Timer multipli simultanei (media del tempo)
- Calcolo automatico "Totale commessa" e "Totale giornata"
- Persistenza dei dati in caso di disconnessione

### Notifiche
- **Pausa Pranzo (12:30)**: Notifica per confermare/fermare tracking
- **Fine Turno (17:30)**: Notifica per confermare/fermare tracking

### Gestione Errori
- Conferma prima di avviare tracking multipli
- Conferma prima di cambiare commessa in tracking
- Validazione dati lato client e server

## Sviluppo

### Backend
```bash
# Esegui i test
php artisan test

# Genera documentazione API
php artisan l5-swagger:generate

# Pulisci cache
php artisan cache:clear
php artisan config:clear
```

### Frontend
```bash
# Esegui i test
npm test

# Build per produzione
npm run build

# Analizza bundle
npm run analyze
```

## Note di Implementazione

### Punti da Definire
- [ ] Gestione pausa: commessa separata o gap temporale
- [ ] Modalità visualizzazione scheda tecnica
- [ ] Gestione notifiche: stop automatico o manuale

### Considerazioni Tecniche
- I dati di tracking sono sincronizzati in tempo reale con il backend
- Il sistema supporta fino a 10 tracking paralleli
- Le password sono cifrate secondo gli standard Laravel
- L'interfaccia è ottimizzata per tablet

## Deployment

### Produzione
```bash
# Backend
php artisan config:cache
php artisan route:cache
php artisan view:cache

# Frontend
npm run build
```

## Contribuire

Sviluppato da **B4web s.r.l.**
- Via Giordano Bruno, 51 - 15121 Alessandria (AL)
- Tel: 0131.1926569
- Email: info@b4web.biz
- Web: www.b4web.biz

## Licenza

Proprietario: Italgold  
Sviluppatore: B4web s.r.l.

---

Per maggiori informazioni, consultare la documentazione completa o contattare il team di sviluppo.
