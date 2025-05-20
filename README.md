# Debate.io Backend

A FastAPI-based backend service for a real-time debate platform with AI-powered topic generation and automated argument scoring.

---

## Features

* Real-time debate room management
* AI-powered topic generation using the Gemini API
* Automated argument scoring
* History storage using MinIO
* Player management and ranking system

---

## Prerequisites

* Docker & Docker Compose (recommended)
* Python 3.9 or higher
* pip package manager

---

## Installation

### Option 1: Docker Setup (Recommended)

1. Clone the repository

   ```bash
   git clone https://github.com/sayan-does/debate.io.git
   cd debate.io/backend
   ```

2. Create a `.env` file

   ```env
   GEMINI_API_KEY=your_gemini_api_key_here
   ```

3. Start services

   ```bash
   docker-compose up -d
   ```

---

### Option 2: Local Development

1. Set up a virtual environment

   ```bash
   # Unix/macOS
   python3 -m venv venv
   source venv/bin/activate

   # Windows
   python -m venv venv
   .\venv\Scripts\activate
   ```

2. Install dependencies

   ```bash
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

3. Create a `.env` file

   ```env
   MINIO_ENDPOINT=localhost:9000
   MINIO_ACCESS_KEY=sayan
   MINIO_SECRET_KEY=admin123
   GEMINI_API_KEY=your_gemini_api_key_here
   ```

4. Start MinIO server

   ```bash
   docker run -d \
     -p 9000:9000 -p 9001:9001 \
     -e "MINIO_ROOT_USER=YOUR_MINIO_USER_NAME" \
     -e "MINIO_ROOT_PASSWORD=PASSWORD" \
     -v minio_data:/data \
     minio/minio server /data --console-address ":9001"  
   ```

5. Run the application

   ```bash
   uvicorn main:app --host 0.0.0.0 --port 8000 --reload
   ```

---

## Access Points

* Backend API: [http://localhost:8000](http://localhost:8000)
* MinIO Console: [http://localhost:9001](http://localhost:9001)


---

## API Endpoints

### Player Management

* `POST /players/create` - Create a new player
* `GET /players/{username}` - Get player details
* `GET /player/history/{username}` - Get player history and rankings

### Debate Topics

* `GET /genres` - Get available debate genres
* `GET /topics/{genre}` - Get debate topics by genre

### Room Management

* `POST /create-room/{player_name}?topic={topic}` - Create a new debate room
* `POST /join-room/{room_key}` - Join an existing room
* `POST /submit-argument/{room_key}/{player_name}` - Submit an argument
* `POST /abort-debate/{room_key}/{player_name}` - Abort a debate
* `GET /room-status/{room_key}` - Get current room status

---

## Configuration

### Environment Variables

```env
# MinIO
MINIO_ENDPOINT=localhost:9000
MINIO_ACCESS_KEY=USER_NAME
MINIO_SECRET_KEY=KEY_GIVEN_BY_MINIO

# Gemini API
GEMINI_API_KEY=your_gemini_api_key_here
```

---

## Docker Health Checks

* API health check interval: 30 seconds
* MinIO health check interval: 30 seconds
* Retry attempts: 3

---

## Troubleshooting

### MinIO Connection Issues

* Verify credentials in `.env`
* Ensure ports 9000 and 9001 are open
* View logs using:

  ```bash
  docker logs debate-game-minio
  ```

### Gemini API Issues

* Check the `GEMINI_API_KEY` value
* Ensure the API key has the correct permissions and available quota
