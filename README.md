# ✈️ FPL VFR - Flight Planner for VFR Flights

> Kompleksowa aplikacja webowa do planowania lotów VFR (Visual Flight Rules) z automatycznymi obliczeniami nawigacyjnymi, integracją danych pogodowych i generowaniem dokumentacji lotniczej.

![Next.js](https://img.shields.io/badge/Next.js-14-black?logo=next.js)
![NestJS](https://img.shields.io/badge/NestJS-10-red?logo=nestjs)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-blue?logo=postgresql)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-3-orange?logo=rabbitmq)
![TypeScript](https://img.shields.io/badge/TypeScript-5-blue?logo=typescript)
![Docker](https://img.shields.io/badge/Docker-Ready-blue?logo=docker)

---

## 📋 Spis treści

- [Funkcjonalności](#-funkcjonalności)
- [Stack technologiczny](#-stack-technologiczny)
- [Architektura](#-architektura)
- [Instalacja](#-instalacja-i-uruchomienie)
- [Struktura projektu](#-struktura-projektu)
- [API](#-api-endpoints)
- [Baza danych](#-baza-danych)
- [Wzory nawigacyjne](#-wzory-nawigacyjne)
- [Autor](#-autor)

---

## 🚀 Funkcjonalności

### Zaimplementowane (100%)

| Funkcja | Opis | Status |
|---------|------|--------|
| **Interaktywna mapa** | Leaflet z waypointami, strefami GAMET/AUP | ✅ |
| **Obliczenia nawigacyjne** | TC, MC, WCA, HDG, GS, ETE automatycznie | ✅ |
| **Pogoda METAR/TAF** | Integracja z aviationweather.gov | ✅ |
| **Cache pogody** | Bulk METAR z 30-min cache w pamięci | ✅ |
| **GAMET** | Strefy A1-A5 FIR Warszawa z wiatrem | ✅ |
| **NOTAMs** | Wyszukiwanie dla lotnisk i FIR | ✅ |
| **Weight & Balance** | Kalkulator z wykresem CG envelope | ✅ |
| **Baza lotnisk** | 50+ polskich lotnisk (IFR/VFR/MIL) | ✅ |
| **Generator PDF** | OPL w formacie polskim (landscape) | ✅ |
| **Zarządzanie flotą** | Profile samolotów z danymi W&B | ✅ |
| **Autentykacja JWT** | Rejestracja, logowanie, sesje | ✅ |
| **Asynchroniczność** | RabbitMQ + CRON dla pogody | ✅ |
| **Responsywność** | Mobile-first design (Tailwind) | ✅ |
| **Swagger API** | Pełna dokumentacja OpenAPI | ✅ |

---

## 🛠 Stack technologiczny

### Frontend
| Technologia | Uzasadnienie |
|-------------|--------------|
| **Next.js 14** | App Router, SSR, optymalizacja bundle |
| **React 18** | Hooks, Server Components |
| **TypeScript** | Type safety, lepsze DX |
| **Tailwind CSS** | Utility-first, responsywność |
| **Zustand** | Lekki state management |
| **React Query** | Cache i synchronizacja danych |
| **Leaflet** | Interaktywne mapy lotnicze |

### Backend
| Technologia | Uzasadnienie |
|-------------|--------------|
| **NestJS 10** | Modułowa architektura, DI, decorators |
| **Prisma ORM** | Type-safe queries, migracje |
| **PostgreSQL 16** | Relacyjna baza, JSON support |
| **RabbitMQ** | Message queue dla async tasks |
| **Passport.js** | JWT authentication |
| **Swagger** | Auto-generowana dokumentacja API |
| **PDFKit** | Generowanie dokumentów PDF |

### DevOps
| Technologia | Uzasadnienie |
|-------------|--------------|
| **Docker** | Containerization |
| **Docker Compose** | Multi-container orchestration |

---

## 🏗 Architektura

### Diagram wysokopoziomowy

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│                 │     │                 │     │                 │
│    Frontend     │────▶│    Backend      │────▶│   PostgreSQL    │
│    (Next.js)    │     │    (NestJS)     │     │                 │
│    Port 3000    │     │    Port 3001    │     │    Port 5432    │
│                 │     │                 │     │                 │
└─────────────────┘     └────────┬────────┘     └─────────────────┘
                                 │
                                 │
                                 ▼
                        ┌─────────────────┐
                        │                 │
                        │    RabbitMQ     │
                        │  (Async Tasks)  │
                        │    Port 5672    │
                        │                 │
                        └─────────────────┘
```

### Architektura kodu (warstwy)

```
Backend (NestJS)
├── Controllers    → Obsługa HTTP requests, walidacja, routing
├── Services       → Logika biznesowa, integracje z zewnętrznymi API
├── Prisma         → Warstwa dostępu do danych (Repository pattern)
├── Guards         → Autoryzacja (JWT)
├── DTOs           → Walidacja danych wejściowych (class-validator)
└── Modules        → Enkapsulacja funkcjonalności (DI)
```

### Diagram ERD

Pełny diagram ERD znajduje się w pliku [`docs/ERD.md`](docs/ERD.md)

**Tabele (9):** User, Aircraft, Airport, Runway, FlightPlan, Waypoint, FlightLeg, WeatherData, NotamData

---

## 💻 Instalacja i uruchomienie

### Wymagania

- Docker & Docker Compose
- Node.js 20+ (opcjonalnie, dla developmentu)

### 1. Klonowanie repozytorium

```bash
git clone <repository-url>
cd ZTPAI
```

### 2. Konfiguracja środowiska

```bash
# Backend
cp backend/.env.example backend/.env

# Frontend  
cp frontend/.env.example frontend/.env
```

### 3. Uruchomienie (Docker)

```bash
docker-compose up --build
```

### 4. Inicjalizacja bazy danych

```bash
# Wejdź do kontenera backend
docker-compose exec backend sh

# Migracje Prisma
npx prisma migrate dev

# Załaduj dane początkowe (50+ lotnisk, samoloty demo)
npx prisma db seed
```

### 5. Dostęp do aplikacji

| Serwis | URL | Dane logowania |
|--------|-----|----------------|
| **Frontend** | http://localhost:3000 | - |
| **Backend API** | http://localhost:3001 | - |
| **Swagger Docs** | http://localhost:3001/api | - |
| **RabbitMQ** | http://localhost:15672 | guest / guest |

**Demo user:** `pilot@demo.com` / `demo123`

---

## 📁 Struktura projektu

```
ZTPAI/
├── docker-compose.yml          # Orchestracja kontenerów
├── docs/
│   └── ERD.md                  # Diagram ERD (Mermaid)
├── backend/
│   ├── Dockerfile
│   ├── prisma/
│   │   ├── schema.prisma       # Schemat bazy (9 tabel)
│   │   └── seed.ts             # Dane początkowe (50+ rekordów)
│   └── src/
│       ├── main.ts             # Bootstrap + Swagger
│       ├── app.module.ts       # Root module
│       ├── prisma/             # Prisma service
│       ├── rabbitmq/           # RabbitMQ config
│       └── modules/
│           ├── auth/           # JWT auth (guards, strategies)
│           ├── users/          # User CRUD
│           ├── aircraft/       # Aircraft + W&B calculations
│           ├── airports/       # Airport database
│           ├── weather/        # METAR/TAF/GAMET API
│           ├── weather-queue/  # RabbitMQ async weather
│           ├── notam/          # NOTAM API
│           ├── calculations/   # Navigation formulas
│           ├── flight-plans/   # Flight plan CRUD
│           └── pdf/            # OPL PDF generator
└── frontend/
    ├── Dockerfile
    └── src/
        ├── app/                # Next.js App Router pages
        │   ├── dashboard/
        │   ├── planner/        # Interactive map
        │   ├── weather/
        │   ├── notam/
        │   ├── airports/
        │   ├── aircraft/
        │   └── calculator/
        ├── components/
        │   ├── FlightMap.tsx   # Leaflet map
        │   ├── NavigationLog.tsx
        │   ├── WeatherPanel.tsx
        │   └── WeightBalance.tsx
        ├── store/              # Zustand stores
        └── lib/                # API client (axios)
```

---

## 🔌 API Endpoints

### Authentication
| Method | Endpoint | Opis |
|--------|----------|------|
| POST | `/auth/register` | Rejestracja użytkownika |
| POST | `/auth/login` | Logowanie (zwraca JWT) |

### Weather
| Method | Endpoint | Opis |
|--------|----------|------|
| GET | `/weather/metar/:icao` | METAR dla lotniska |
| GET | `/weather/metars/all` | Wszystkie METARy (cache) |
| GET | `/weather/taf/:icao` | TAF dla lotniska |
| GET | `/weather/gamet` | GAMET FIR Warszawa |
| GET | `/weather/briefing/:dep/:arr` | Weather briefing |

### Weather Queue (RabbitMQ)
| Method | Endpoint | Opis |
|--------|----------|------|
| POST | `/weather-queue/refresh/:icao` | Kolejkuj METAR refresh |
| GET | `/weather-queue/stats` | Statystyki kolejki |

### NOTAM
| Method | Endpoint | Opis |
|--------|----------|------|
| GET | `/notam/:icao` | NOTAMs dla lokacji |
| GET | `/notam/:icao/important` | Ważne NOTAMs |
| GET | `/notam/fir/poland` | NOTAMs FIR Poland |

### Calculations
| Method | Endpoint | Opis |
|--------|----------|------|
| POST | `/calculations/leg` | Obliczenia dla odcinka |
| POST | `/calculations/route` | Obliczenia dla trasy |
| GET | `/calculations/distance` | Dystans między punktami |

### Aircraft
| Method | Endpoint | Opis |
|--------|----------|------|
| GET | `/aircraft` | Lista samolotów |
| POST | `/aircraft` | Dodaj samolot |
| POST | `/aircraft/:id/weight-balance` | Oblicz W&B |

### Flight Plans
| Method | Endpoint | Opis |
|--------|----------|------|
| GET | `/flight-plans` | Lista planów |
| POST | `/flight-plans` | Utwórz plan |
| POST | `/flight-plans/:id/waypoints` | Dodaj waypoint |

### PDF
| Method | Endpoint | Opis |
|--------|----------|------|
| GET | `/pdf/opl/:flightPlanId` | Pobierz OPL (PDF) |

---

## 🗄 Baza danych

### Tabele (9)

1. **users** - Użytkownicy systemu
2. **aircraft** - Profile samolotów z danymi W&B
3. **airports** - Lotniska (50+ polskich)
4. **runways** - Drogi startowe
5. **flight_plans** - Plany lotu
6. **waypoints** - Punkty trasy
7. **flight_legs** - Odcinki z obliczeniami
8. **weather_data** - Cache METAR/TAF
9. **notam_data** - Cache NOTAMs

### Normalizacja

Baza jest w **3NF (trzeciej postaci normalnej)**:
- Brak powtarzających się grup (1NF)
- Pełna zależność funkcyjna od klucza (2NF)
- Brak zależności przechodnich (3NF)

### Dane początkowe (seed)

- **50+ lotnisk** polskich (IFR, VFR, wojskowe)
- **2 profile samolotów** (C152, PA28)
- **1 użytkownik demo**
- **Razem: ~55 rekordów**

---

## 🧮 Wzory nawigacyjne

### Dystans (Haversine)
```
d = 2R × arcsin(√(sin²(Δlat/2) + cos(lat1)×cos(lat2)×sin²(Δlon/2)))
```

### True Course
```
TC = atan2(sin(Δlon)×cos(lat2), cos(lat1)×sin(lat2) - sin(lat1)×cos(lat2)×cos(Δlon))
```

### Wind Correction Angle
```
WCA = arcsin((Vw/TAS) × sin(WD - TC))
```

### Ground Speed
```
GS = TAS × cos(WCA) + Vw × cos(WD - TC)
```

### Magnetic Heading
```
MH = TC - VAR + WCA
```

---

## 🐰 Asynchroniczność (RabbitMQ)

Moduł `weather-queue` demonstruje użycie kolejek:

### Publikowanie zadań
```typescript
// Kolejkowanie odświeżenia METAR
await weatherQueueService.queueMetarRefresh('EPWA', 'high');

// Kolejkowanie briefingu pogodowego
await weatherQueueService.queueFlightPlanBriefing(
  flightPlanId, 'EPWA', 'EPKK'
);
```

### CRON jobs
```typescript
@Cron(CronExpression.EVERY_30_MINUTES)
async scheduleMetarRefresh() {
  // Automatyczne odświeżanie METARów co 30 minut
}
```

### Konsumenci
```typescript
@EventPattern('weather.refresh.metar')
async handleMetarRefresh(message: WeatherRefreshMessage) {
  await this.processMetarRefresh(message);
}
```

---

## 🔐 Bezpieczeństwo

- **JWT** tokens z expiration
- **bcrypt** password hashing
- **Guards** na chronionych endpointach
- **DTOs** z class-validator
- **CORS** configuration

---

## 🔧 Development

### Backend (bez Dockera)
```bash
cd backend
npm install
npx prisma generate
npx prisma migrate dev
npm run start:dev
```

### Frontend (bez Dockera)
```bash
cd frontend
npm install
npm run dev
```

---

## 📝 Licencja

MIT License

---

## 👨‍💻 Autor

Projekt stworzony na potrzeby przedmiotu **ZTPAI** (Zaawansowane Technologie Projektowania Aplikacji Internetowych).

---

## 📊 Checklist wymagań

| # | Wymaganie | Status |
|---|-----------|--------|
| 1 | README i instrukcja | ✅ |
| 2 | Diagram ERD (9 tabel) | ✅ |
| 3 | Baza w 3NF, 30+ rekordów | ✅ |
| 4 | 40+ commitów | ⚠️ Do weryfikacji |
| 5 | 70% funkcjonalności | ✅ 100% |
| 6 | Nowoczesne technologie | ✅ |
| 7 | Architektura warstwowa | ✅ |
| 8 | UX/UI responsywne | ✅ |
| 9 | JWT + autoryzacja | ✅ |
| 10 | REST API zgodne | ✅ |
| 11 | Frontend korzysta z API | ✅ |
| 12 | Jakość kodu | ✅ |
| 13 | RabbitMQ async | ✅ |
| 14 | Swagger docs | ✅ |
