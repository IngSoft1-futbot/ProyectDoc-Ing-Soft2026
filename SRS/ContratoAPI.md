# 3. API Specification

## 3.1 Overview

The Futbot API is a RESTful web service built with FastAPI that provides endpoints for all core functionalities of the football game management system. The API follows standard REST conventions and supports JSON data exchange.

## 3.2 Authentication

All API endpoints require authentication via JWT tokens. Tokens are obtained through the `/auth/login` endpoint and must be included in the Authorization header as `Bearer <token>` for protected routes.

## 3.3 Base URL
```
https://api.futbot.com/v1/
```

## 3.4 Common Response Formats

### Success Response
```json
{
  "status": "200",
  "data": {},
  "message": "Operation completed successfully"
}
```

### Error Response
```json
{
  "status": "error",
  "error": {
    "code": "ERROR_CODE",
    "message": "Error description"
  }
}
```

## 3.5 API Endpoints

### Authentication Endpoints

#### 3.5.1 User Registration
**POST** `/auth/register`

**Request Body:**
```json
{
  "username": "string",
  "email": "string",
  "password": "string",
  "name": "string",
  "avatar": "string"
}
```

**Response:**
```json
{
  "status": "200",
  "data": {
    "user_id": "integer",
    "username": "string",
    "email": "string",
    "name": "string"
  },
  "message": "User registered successfully"
}
```

#### 3.5.2 User Login
**POST** `/auth/login`

**Request Body:**
```json
{
  "email": "string",
  "password": "string"
}
```

**Response:**
```json
{
  "status": "200",
  "data": {
    "access_token": "string",
    "token_type": "bearer"
  },
  "message": "Login successful"
}
```

### User Management Endpoints

#### 3.5.3 Get User Profile
**GET** `/users/profile`

**Response:**
```json
{
  "status": "200",
  "data": {
    "user_id": "integer",
    "username": "string",
    "email": "string",
    "name": "string",
    "avatar": "string",
    "created_at": "datetime"
  },
  "message": "Profile retrieved successfully"
}
```

#### 3.5.4 Update User Profile
**PUT** `/users/profile`

**Request Body:**
```json
{
  "name": "string",
  "email": "string",
  "avatar": "string",
  "old_password": "string",
  "new_password": "string"
}
```

**Response:**
```json
{
  "status": "200",
  "data": {
    "user_id": "integer",
    "username": "string",
    "email": "string",
    "name": "string",
    "avatar": "string"
  },
  "message": "Profile updated successfully"
}
```

### Player Management Endpoints

#### 3.5.5 Create Player
**POST** `/players`

**Request Body:**
```json
{
  "name": "string",
  "pacss_attributes": {
    "power": "integer",
    "speed": "integer",
    "dexterity": "integer",
    "control": "integer",
    "strength": "integer"
  },
  //dorsal
  "team_id": "integer"
}
```

**Response:**
```json
{
  "status": "200",
  "data": {
    "player_id": "integer",
    "name": "string",
    "pacss_attributes": {
      "power": "integer",
      "speed": "integer",
      "dexterity": "integer",
      "control": "integer",
      "strength": "integer"
    },
    "team_id": "integer"
  },
  "message": "Player created successfully"
}
```

#### 3.5.6 Get Player
**GET** `/players/{player_id}`

**Response:**
```json
{
  "status": "200",
  "data": {
    "player_id": "integer",
    "name": "string",
    "pacss_attributes": {
      "power": "integer",
      "speed": "integer",
      "dexterity": "integer",
      "control": "integer",
      "strength": "integer"
    },
    "team_id": "integer"
  },
  "message": "Player retrieved successfully"
}
```

#### 3.5.7 Update Player    //delete
**PUT** `/players/{player_id}`

**Request Body:**
```json
{
  "name": "string",
  "pacss_attributes": {
    "power": "integer",
    "speed": "integer",
    "dexterity": "integer",
    "control": "integer",
    "strength": "integer"
  }
}
```

**Response:**
```json
{
  "status": "200",
  "data": {
    "player_id": "integer",
    "name": "string",
    "pacss_attributes": {
      "power": "integer",
      "speed": "integer",
      "dexterity": "integer",
      "control": "integer",
      "strength": "integer"
    }
  },
  "message": "Player updated successfully"
}
```

### Team Management Endpoints

#### 3.5.8 Create Team
**POST** `/teams`

**Request Body:**
```json
{
  "name": "string",
  "player_ids_titulares": ["integer"],
  "player_ids_suplentes": ["integer"]
}
```

**Response:**
```json
{
  "status": "200",
  "data": {
    "team_id": "integer",
    "name": "string",
    "player_ids_titulares": [
      {
        "player_id": "integer",
        "name": "string"
      },
      {
        "player_id": "integer",
        "name": "string"
      },
      {
        "player_id": "integer",
        "name": "string"
      }
    ],
    "player_ids_suplentes": [
      {
        "player_id": "integer",
        "name": "string"
      },
      {
        "player_id": "integer",
        "name": "string"
      },
      {
        "player_id": "integer",
        "name": "string"
      }
    ]
  },
  "message": "Team created successfully"
}
```

#### 3.5.9 Get Team
**GET** `/teams/{team_id}`

**Response:**
```json
{
  "status": "200",
  "data": {
    "team_id": "integer",
    "name": "string",
    "players": [
      {
        "player_id": "integer",
        "name": "string",
        "pacss_attributes": {
          "power": "integer",
          "speed": "integer",
          "dexterity": "integer",
          "control": "integer",
          "strength": "integer"
        }
      }
    ]
  },
  "message": "Team retrieved successfully"
}
```

### League Management Endpoints

#### 3.5.10 Create League
**POST** `/leagues`

**Request Body:**
```json
{
  "name": "string",
  "is_private": "boolean",
  "password": "string",
  "duration": "integer",
  "min_teams": "integer",
  "max_teams": "integer",
  "fecha_inicio": "integer",
  "incritos": "[Equipo]"
}
```

**Response:**
```json
{
  "status": "200",
  "data": {
    "league_id": "integer",
    "name": "string",
    "is_private": "boolean"
  },
  "message": "League created successfully"
}
```

#### 3.5.11 Get League
**GET** `/leagues/{league_id}`

**Response:**
```json
{
  "status": "200",
  "data": {
    "league_id": "integer",
    "name": "string",
    "is_private": "boolean",
    "teams": [
      {
        "team_id": "integer",
        "name": "string"
      }
    ],
    "matches": [
      {
        "match_id": "integer",
        "home_team": "string",
        "away_team": "string",
        "scheduled_at": "datetime"
      }
    ]
  },
  "message": "League retrieved successfully"
}
```

### Behavior Management Endpoints

#### 3.5.12 Create Behavior
**POST** `/behaviors`

**Request Body:**
```json
{
  "id": "integer",
  "name": "string",
  "code": "string"
}
```

**Response:**
```json
{
  "status": "200",
  "data": {
    "behavior_id": "integer",
    "name": "string",
    "description": "string"
  },
  "message": "Behavior created successfully"
}
```

#### 3.5.13 Get Behavior
**GET** `/behaviors/{behavior_id}`

**Response:**
```json
{
  "status": "200",
  "data": {
    "behavior_id": "integer",
    "name": "string",
    "description": "string",
    "code": "string"
  },
  "message": "Behavior retrieved successfully"
}
```

### Match Management Endpoints

#### 3.5.14 Schedule Match
**POST** `/matches/schedule`

**Request Body:**
```json
{
  "league_id": "integer",
  "home_team_id": "integer",
  "away_team_id": "integer",
  "scheduled_at": "datetime"
}
```

**Response:**
```json
{
  "status": "200",
  "data": {
    "match_id": "integer",
    "league_id": "integer",
    "home_team": "string",
    "away_team": "string",
    "scheduled_at": "datetime"
  },
  "message": "Match scheduled successfully"
}
```

#### 3.5.15 Get Match
**GET** `/matches/{match_id}`

**Response:**
```json
{
  "status": "200",
  "data": {
    "match_id": "integer",
    "league_id": "integer",
    "home_team": "string",
    "away_team": "string",
    "scheduled_at": "datetime",
    "status": "string",
    "results": {
      "home_score": "integer",
      "away_score": "integer"
    }
  },
  "message": "Match retrieved successfully"
}
```
## 4. Real-Time Communication (WebSockets)

To enable live updates during matches, the system supports WebSocket connections in addition to REST endpoints. All WebSocket connections must include a valid JWT token in the `Authorization` header as `Bearer <token>`, matching the standard REST authentication method defined in Section **3.2**.

### 4.1 WebSocket Endpoint
- **Endpoint:** `/matches/{match_id}/websocket`
- **Protocol:** WebSocket (RFC 6455)
- **Base Domain:** The connection path is relative to the application's domain (e.g., `wss://api.futbot.com/v1/matches/{match_id}/websocket`).

### 4.1 Server-to-Client Messages
The server broadcasts events to all connected clients (spectators/coaches) during a match.

| Event Type | Description | Data Example |
|-------------|-------------|--------------|
| `match_update` | General match state update (score, time, quarter) | ```json {"type": "match_update", "data": {"score": "1-0", "time": "15:00", "quarter": 1}}``` |
| `match_event` | A specific event occurred (goal, behavior switches, player switches) | ```json {"type": "match_event", "data": {"event": "goal", "player": "Player A", "minute": 12}}``` |
| `substitution_event` | A substitution was made | ```json {"type": "substitution_event", "data": {"player_out": "Player X", "player_in": "Player Y"}}``` |

### 4.1 Client-to-Server Messages
Clients can send specific commands during a match.

| Event Type | Description | Data Example |
|-------------|-------------|--------------|
| `substitution_request` | Request to perform a substitution | ```json {"type": "substitution_request", "player_out_id": 1, "player_in_id": 2}``` |

---

## 5. Standard HTTP Status Codes

While custom error codes are returned in the JSON payload (as defined in Section **3.6 Error Codes**), the API uses standard HTTP status codes to indicate the outcome of requests.

| Code | Description |
|------|-------------|
| `200 OK` | Request successful. |
| `201 Created` | Resource created successfully. |
| `400 Bad Request` | Invalid input or business rule violation (e.g., insufficient PACSS points). |
| `401 Unauthorized` | Authentication required or invalid credentials. |
| `403 Forbidden` | Permission denied. |
| `404 Not Found` | Resource not found. |
| `500 Internal Server Error` | Server-side error occurred. |
```