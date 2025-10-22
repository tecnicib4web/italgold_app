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
- **Autenticazione**: Laravel Pasport (OAuth)
- **Broadcasting**: Reverb (gestione pause ore 12:30)

### Frontend
- **Framework**: React
- **Routing**: React Router
- **UI**: Tailwind CSS
- **HTTP Client**: Axios
- **Timer Management**: Custom hooks per gestione cronometri
- **Broadcasting**: Echo

## Requisiti di Sistema

### Backend
- PHP >= 8.3
- Node.js >= 22.x (per compilazione asset)

### Frontend
- Node.js >= 22.x
- npm

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
APP_KEY=Chiave dell'app
APP_URL=http://localhost:8000

DB_CONNECTION=sqlsrv
DB_HOST=192.168.0.72
DB_PORT=1433
DB_DATABASE=Tablet
DB_USERNAME=php_user
DB_PASSWORD=
DB_ENCRYPT=yes
DB_TRUST_SERVER_CERTIFICATE=true

REVERB_APP_ID=312182
REVERB_APP_KEY=Key
REVERB_APP_SECRET= Secret
REVERB_HOST=192.168.0.7
REVERB_PORT=8080
REVERB_SCHEME=http
REVERB_SERVER_HOST='0.0.0.0'
REVERB_SERVER_PORT=8080

...
```

### Variabili d'Ambiente Frontend (.env)

```env
REACT_APP_API_URL=http://localhost:8000/api
REACT_APP_NAME=Italgold
```

## Struttura del Progetto

```
backend
├───app
│   ├───Console
│   │   └───Commands
│   ├───Events
│   ├───Helpers
│   ├───Http
│   │   └───Controllers
│   │       └───Api
│   ├───Jobs
│   ├───Models
│   └───Providers
├───bootstrap
│   └───cache
├───config
├───database
│   ├───factories
│   ├───migrations
│   └───seeders
├───node_modules
│   └── ...
└── ...

frontend
└───src
    ├───common
    │   └───Loader
    ├───components
    │   ├───Header
    │   ├───modals
    │   └───Tables
    ├───css
    ├───fonts
    ├───hooks
    ├───images
    │   └───product
    ├───lib
    ├───pages
    ├───types
    └───utils
    └── ...
|___...    
```

## API Endpoints

### Autenticazione
-Tutte le route protette richiedono 'header -> Authorization: Bearer <token>

## Route pubbliche
- `POST /api/login` - Login utente

## Route protette
- `POST /api/logout` - Logout utente
- `GET /api/me` - Informazioni utente autenticato

### Commesse
# Commesse assegnate a una risorsa
- `GET /api/base/commesse-per-risorsa` - 
Query Param: risorsa_id (opzionale)

# File allegati a una commessa
- `GET /api/base/commessa-files` - 
Query Param: commessa_id (opzionale)


### Tracking
# Avvia una o più lavorazioni
- `POST /api/lavorazione/registra-inizio-lavorazione` - 
Body(singolo): { "dcl_id": 1, "risorsa_id": 123, "commessa_id": 456 }
Body(batch): { "items": [{ "dcl_id": 1, "risorsa_id": 123, "commessa_id": 456 }] }

# Chiude una o più lavorazioni
- `POST /api/lavorazione/registra-fine-lavorazione` - 
Body(singolo): { "tempo_id": 10, "sec_tempo": 3600, "nr_pezzi": 5, "nr_pietre": 2 }, 
Body(batch): { "items": [{ "tempo_id": 10, "sec_tempo": 3600, "nr_pezzi": 5, "nr_pietre": 2 }] }

# Statistiche giornaliere per risorsa/data
- `GET /api/base/statistiche-giornaliere` - 
Query Param: risorsa_id (opzionale), data (YYYY-MM-DD, opzionale, se non fornito fallback = giornata odierna)

# Tempo totale per commessa per risorsa
- `GET /api/base/tempo-totale-per-commessa` – 
Query Param: risorsa_id (opzionale), include_ongoing (0|1, opzionale)

# Lavorazioni attive per risorsa
- `GET /api/base/lavorazioni-attive` - 
Query Param: risorsa_id (opzionale)


## Funzionalità Chiave

### Time Tracking
Il sistema gestisce il tracking del tempo attraverso:
- Timer singolo per commessa
- Timer multipli simultanei (media del tempo)
- Calcolo automatico "Totale commessa" e "Totale giornata"
- Persistenza dei dati in caso di disconnessione

### Notifiche
## Il server tramite un canale privato(Reverb) invia il messaggio ai tablet che sono online e che stanno registrando su una commessa
- **Pausa Pranzo (12:30)**: Notifica per confermare/fermare tracking
- **Fine Turno (17:30)**: Notifica per confermare/fermare tracking

### Stato tablet(Admin)
## L'amministratore potra vedere quale tablet e online con una query alla DB
- Sul server ci sara un cron che ogni tot minuti andrà a controllare se raggiunge tutti i tablet e settare il loro stato (ON/OFF)

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

## Licenza

Sviluppato da **B4web s.r.l.**
- Via Giordano Bruno, 51 - 15121 Alessandria (AL)
- Tel: 0131.1926569
- Email: info@b4web.biz
- Web: www.b4web.biz

Proprietario: Italgold  
Sviluppatore: B4web s.r.l.

---

Per maggiori informazioni, consultare la documentazione completa o contattare il team di sviluppo.
