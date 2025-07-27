# Appvinio Survey App

## Prerequisites

- **Docker** and **Docker Compose** installed
- **Java 21** (only needed to build the API)
- **Node.js 18+** (Docker will handle this, but helpful for local development)


This repository serves as the guide to run and configure the [AppvSurveyClient](https://github.com/DarknessoPirate/AppvSurveyClient) and [AppvSurveyAPI](https://github.com/DarknessoPirate/AppvSurveyApi) with docker-compose
## 📁 1. Required Folder Structure
### 1. Clone this repository or download the .env and docker-compose files
### 2. Clone the [AppvSurveyClient](https://github.com/DarknessoPirate/AppvSurveyClient) and [AppvSurveyAPI](https://github.com/DarknessoPirate/AppvSurveyApi)
### 3. The structure of your main folder should look like this:
```
Your-Project-Folder/
├── AppvSurveyAPI/          # Spring Boot API
│   ├── build/libs/         # JAR file will be here after build
│   ├── Dockerfile          # Already included
│   └── ... (source code)
├── AppvSurveyClient/       # Angular Frontend  
│   ├── Dockerfile          # Already included
│   └── ... (source code)
├── docker-compose.yml      # Container configuration
└── .env                    # Environment variables
```

## 2. Build the API first
**IMPORTANT:** You must build the Spring Boot API first to create the JAR file.

```bash
cd AppvSurveyAPI
# Build the jar file
./gradlew build

# or skip tests if you have problems building due to database not initialized
./gradlew build -x test
```

## 3. Environment variables
Located in .env file
```bash
# Port Configuration (random port numbers - change if needed)
FRONTEND_EXTERNAL_PORT=38471
API_EXTERNAL_PORT=41259
DB_EXTERNAL_PORT=47832

# Database Configuration (CHANGE THE DEFAULTS !!!)
POSTGRES_DB=SurveyDB
POSTGRES_USER=surveyuser
POSTGRES_PASSWORD=your_secure_password_here

# Application Configuration (CHANGE THE DEFAULTS !!!)
JWT_SECRET=your_long_random_jwt_secret_here_at_least_256_bits
ADMIN_USERNAME=your_admin_username
ADMIN_PASSWORD=your_strong_admin_password
ADMIN_EMAIL=admin@your-domain.com

# Logging 
SHOW_SQL=false          # Show SQL queries in logs (true/false)
LOG_SQL=WARN            # SQL logging level (DEBUG/INFO/WARN/ERROR)  
LOG_APP=INFO            # Application logging level
LOG_ROOT=WARN           # Root logging level
```


## 4. Run
Go into the root folder with docker-compose and .env files
```bash
cd path/to/root/of/project
docker-compose up # just run
# or to run detached in background
docker-compose up -d

# to stop
docker-compose down

```

## To access (with default config - locally)
**Survey App:** [http://localhost:38471](http://localhost:38471)
**API Documentation:** [http://localhost:41259/swagger-ui.html](http://localhost:41259/swagger-ui.html)


## For production

1. **Update Angular environment files** (`AppvSurveyClient/src/environments/environment.ts`):
```typescript
export const environment = {
  production: true,
  apiUrl: 'http://YOUR_SERVER_IP:41259/api',
  frontendUrl: 'http://YOUR_SERVER_IP:38471'
};
```

2. **Update CORS settings** in (`AppvSurveyAPI/src/.../SecurityConfig.kt`)
```kotlin
configuration.allowedOrigins = listOf(
"http://YOUR_SERVER_IP:38471", 
"http://localhost:38471"
)
```


## Useful Commands
```bash
# View running containers
docker-compose ps

# View logs
docker-compose logs

# Rebuild after code changes
docker-compose down
docker-compose build --no-cache # rebuild everything
docker-compose build --no-cache api # rebuild just api
docker-compose build --no-cache frontend # rebuild just client
docker-compose up

# Clean everything and start fresh
docker-compose down -v
docker system prune -f
```

