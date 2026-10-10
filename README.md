# ATALINK2026 - Cápsula Meteorológica Rastreador GPS

Sistema de rastreo y recuperación de cápsulas meteorológicas mediante GPS y SMS, con interfaz web en tiempo real.

## 🎯 Características

- ✅ Recepción de SMS desde rastreador GF-07
- ✅ Parseo automático de coordenadas (LAT/LON)
- ✅ Base de datos geoespacial PostgreSQL + PostGIS
- ✅ Mapa interactivo con historial de ruta
- ✅ API REST para consultas
- ✅ Dashboard en tiempo real
- ✅ Despliegue con Docker

## 🏗️ Arquitectura

```
┌──────────────────────────┐
│  Cápsula + GF-07 (GPS)   │
└────────────┬─────────────┘
             │ (SMS)
             ▼
┌──────────────────────────┐
│   Backend FastAPI        │
│   - Parser SMS           │
│   - API REST             │
│   - PostgreSQL + PostGIS │
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────┐
│  Frontend React          │
│  - Mapa Leaflet          │
│  - Dashboard             │
│  - Historial             │
└──────────────────────────┘
```

## 📁 Estructura del Proyecto

```
ATALINK2026/
├── backend/
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py
│   │   ├── models.py
│   │   ├── schemas.py
│   │   ├── gps_parser.py
│   │   └── database.py
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Map.jsx
│   │   │   └── Dashboard.jsx
│   │   ├── pages/
│   │   │   └── App.jsx
│   │   └── index.css
│   ├── package.json
│   └── Dockerfile
├── docker-compose.yml
└── README.md
```

## 🚀 Inicio Rápido

### Opción 1: Con Docker (Recomendado)

```bash
git clone https://github.com/RainerGRR/ATALINK2026.git
cd ATALINK2026
docker-compose up -d
```

Accede a:
- **Frontend**: http://localhost:3000
- **API**: http://localhost:8000
- **Docs API**: http://localhost:8000/docs

### Opción 2: Local

**Backend:**
```bash
cd backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

**Frontend:**
```bash
cd frontend
npm install
npm start
```

## 📡 API Endpoints

### Recibir SMS
```bash
POST /api/sms
Content-Type: application/json

{
  "from_phone": "+34123456789",
  "text": "IMEI:123456789\nLAT:19.4326N\nLON:99.1332W\nTime:2026-10-10 12:34:56"
}
```

### Obtener último punto
```bash
GET /api/positions/latest
```

### Obtener historial
```bash
GET /api/positions/history?limit=100
```

### Obtener ruta completa
```bash
GET /api/route
```

## 🗄️ Base de Datos

PostgreSQL + PostGIS para manejo de datos geoespaciales.

### Tabla principal: positions
```sql
CREATE TABLE positions (
  id SERIAL PRIMARY KEY,
  device_id VARCHAR(50),
  latitude DECIMAL(10, 8),
  longitude DECIMAL(11, 8),
  accuracy DECIMAL(5, 2),
  timestamp TIMESTAMP DEFAULT NOW(),
  location GEOMETRY(POINT, 4326),
  sms_text TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

## 🔧 Configuración

Crear archivo `.env` en la raíz:

```env
# Database
DB_USER=atalink_user
DB_PASSWORD=your_secure_password
DB_HOST=db
DB_PORT=5432
DB_NAME=atalink_db

# API
API_HOST=0.0.0.0
API_PORT=8000
SECRET_KEY=your_secret_key_here

# Frontend
REACT_APP_API_URL=http://localhost:8000
```

## 📊 Ejemplo de SMS del GF-07

Formato típico que parsea el backend:

```
IMEI:123456789012345
LAT:19.4326N
LON:99.1332W
Accuracy:50
Speed:0
Heading:0
Time:2026-10-10 12:34:56
Battery:95%
```

## 🗺️ Mapas Soportados

- OpenStreetMap (default, libre)
- Mapbox (requiere token)
- Google Maps (requiere API key)

## 🛠️ Stack Tecnológico

| Componente | Tech |
|-----------|------|
| Backend | Python 3.11 + FastAPI |
| BD | PostgreSQL 15 + PostGIS |
| Frontend | React 18 + Leaflet |
| Maps | Leaflet + OpenStreetMap |
| Contenedores | Docker + Docker Compose |
| ORM | SQLAlchemy |
| Validación | Pydantic |

## 📋 Formato de Parseo Soportados

- ✅ `LAT:X.XXXN LON:X.XXXW` (GF-07 estándar)
- ✅ `NMEA ($GPRMC)`
- ✅ Coordenadas decimales
- ✅ N/S para latitud, E/W para longitud

## 🔐 Seguridad

- [ ] Autenticación de dispositivos
- [ ] Validación de SMS
- [ ] Rate limiting en endpoints
- [ ] HTTPS en producción
- [ ] Cifrado de datos sensibles

## 📝 Próximos Pasos

1. Configurar PostgreSQL + PostGIS
2. Conectar módem GSM
3. Testear parseo de SMS
4. Implementar notificaciones en tiempo real (WebSocket)
5. Agregar alertas de proximidad
6. Dashboard de telemetría meteorológica

## 🤝 Contribuciones

Este proyecto es parte del equipo de globos meteorológicos ATALINK2026.

## 📧 Contacto

RainerGRR - [GitHub](https://github.com/RainerGRR)

---

**Última actualización**: 2026-10-10
**Estado**: 🚧 En desarrollo
