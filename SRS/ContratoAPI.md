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
  "status": "Code",
  "data": {},
  "message": "Operation completed successfully."
}
```

### Error Response
```json
{
  "status": "error_code",
  "message": "error description"
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
  "status": "201",
  "data": {
    "user_id": "integer",
    "username": "string",
    "email": "string",
    "name": "string"
  },
  "message": "User registered successfully."
}
```
**Expected Errors:**
```json
{
  "status": "400 Bad Request",
  "message": "Email or username already in use."
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
  "message": "Login successful."
}
```

**Expected Errors:**
```json
{
  "status": "401 Unauthorized",
  "message": "Invalid email or password."
}
```

### User Management Endpoints

#### 3.5.3 List Users
**GET**  `/users`
**Response:**
```json
{
  "status": "200",
  "data": [
    {
      "user_id": "integer",
      "name": "string",
      "avatar": "string"
    }
  ],
  "message": "Users listed successfully."
}
```

#### 3.5.4 Get User Profile
**GET** `/users/{user_id}/profile`

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
  "message": "Profile retrieved successfully."
}
```

**Expected Error:**
```json
{
  "status": "404 Not Found",
  "message": "User does not exist."
}
```

#### 3.5.5 Update User Profile
**PUT** `/users/{user_id}/profile`

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
  "message": "Profile updated successfully."
}
```
**Expected Error:**
```json
{
  "status": "401 Unauthorized",
  "message": "Invalid or missing token."
}
```

```json
{
  "status": "403 Forbidden",
  "message": "Attempting to update another user's profile."
}
```

### Player Management Endpoints

#### 3.5.6 Create Player
**POST** `/users/{user_id}/players`

**Request Body:**
```json
{
  "name": "string",
  "shirt_numb": "integer",
  "pacss_attributes": {
    "power": "integer",
    "agility": "integer",
    "control": "integer",
    "speed": "integer",
    "strength": "integer"
  },
  "team_id": "integer"
}
```

**Response:**
```json
{
  "status": "201",
  "data": {
    "player_id": "integer",
    "name": "string",
    "shirt_numb": "integer",
    "pacss_attributes": {
      "power": "integer",
      "agility": "integer",
      "control": "integer",
      "speed": "integer",
      "strength": "integer"
    },
    "team_id": "integer"
  },
  "message": "Player created successfully."
}
```

**Expected Error:**
```json
{
  "status": "400 Bad Request",
  "message": "PACSS attributes exceed the maximum allowed points."
}
```

```json
{
  "status": "404 Not Found",
  "message": "Team does not exist."
}
```

#### 3.5.7 Get Player
**GET** `/users/{user_id}/players/{player_id}`

**Response:**
```json
{
  "status": "200",
  "data": {
    "player_id": "integer",
    "name": "string",
    "pacss_attributes": {
      "power": "integer",
      "agility": "integer",
      "control": "integer",
      "speed": "integer",
      "strength": "integer"
    },
    "team_id": "integer"
  },
  "message": "Player retrieved successfully."
}
```

**Expected Error:**
```json
{
  "status": "404 Not Found",
  "message": "Player not found."
}
```

#### 3.5.8 Delete Player
**DELETE** `/users/{user_id}/players/{player_id}`

**Response:**
```json
{
  "status": "200",
  "message": "Player deleted successfully."
}
```
**Expected Error:**
```json
{
  "status": "403 Forbidden",
  "message": "User does not own the player."
}
```

```json
{
  "status": "404 Not Found",
  "message": "Player not found."
}
```

### Team Management Endpoints

#### 3.5.9 Create Team
**POST** `/users/{user_id}/teams`

**Request Body:**
```json
{
  "name": "string",
  "jugadores_titulares": [
    {
      "player_id": "integer",
      "behavior_id": "integer"
    }
  ],
  "jugadores_suplentes": [
    {
      "player_id": "integer",
      "behavior_id": "integer"
    }
  ]
}
```

**Response:**
```json
{
  "status": "201",
  "data": {
    "team_id": "integer",
    "name": "string",
    "jugadores_titulares": [
      {
        "player_id": "integer",
        "name": "string",
        "behavior_id": "integer"
      }
    ],
    "jugadores_suplentes": [
      {
        "player_id": "integer",
        "name": "string",
        "behavior_id": "integer"
      }
    ]
  },
  "message": "Team created successfully"
}
```

**Expected Error:**
```json
{
  "status": "400 Bad Request",
  "message": "Insufficient number of players to create a team."
}
```

#### 3.5.10 Get Team
**GET** `/users/{user_id}/teams/{team_id}`

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
  "message": "Team retrieved successfully."
}
```

**Expected Error:**
```json
{
  "status": "404 Not Found",
  "message": "Team not found."
}
```

#### 3.5.11 Assign Behavior to Player
**PUT** `/users/{user_id}/teams/{team_id}/players/{player_id}/behavior`

**Request body:**
```json
{
  "behavior_id": "integer"
}
```

**Response:**
```json
{
  "status": "200",
  "data": {
    "player_id": "integer",
    "behavior_id": "integer"
  },
  "message": "Behavior assigned successfully"
}
```

**Expected Errors:**
```json
{
  "status": "404 Not Found",
  "message": "Player or behavior does not exist."
}
```

```json
{
  "status": "409 Conflict",
  "message": "Cannot change behavior while the player is in an active match."
}
```



#### 3.5.12 Delete Team
**DELETE** `/users/{user_id}/teams/{team_id}`

**Response:**
```json
{
  "status": "200",
  "message": "Team deleted successfully."
}
```

**Expected Error:**
```json
{
  "status": "403 Forbidden",
  "message": "User does not own this team."
}
```

```json
{
  "status": "409 Conflict",
  "message": "Cannot delete team while register in an active league."
}
```

### League Management Endpoints

#### 3.5.13 Create League
**POST** `/users/{user_id}/leagues`

**Request Body:**
```json
{
  "name": "string",
  "is_private": "boolean",
  "password": "string",
  "duration": "integer",
  "min_teams": "integer",
  "max_teams": "integer",
  "fecha_inicio": "integer"
}
```

**Response:**
```json
{
  "status": "201",
  "data": {
    "league_id": "integer",
    "name": "string",
    "is_private": "boolean"
  },
  "message": "League created successfully."
}
```

**Expected Error:**
```json
{
  "status": "400 Bad Request",
  "message": "Min teams is greater than Max teams."
}
```

#### 3.5.14 List All Leagues
**GET** `/leagues`

**Response:**
```json
{
  "status": "200",
  "data": [
    {
      "league_id": "integer",
      "name": "string",
      "is_private": "boolean"
    }
  ],
  "message": "Leagues listed successfully."
}
```

#### 3.5.15 List User Leagues
**GET** `/users/{user_id}/leagues`

**Response**
```json
{
  "status": "200",
  "data": [
    {
      "name": "string",
      "league_id": "integer"
    }
  ],
  "message": "Leagues retraived successfully."
}
```

#### 3.5.16 Get User League
**GET** `/users/{user_id}/leagues/{league_id}`

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
  "message": "League retrieved successfully."
}
```

**Expected Error:**
```json
{
  "status": "403 Forbidden",
  "message": "User is not a participant and league is private."
}
```

```json
{
  "status": "404 Not Found",
  "message": "League not found."
}
```
#### 3.5.17 Join League
**POST** `/league/{league_id}/join`

**Request Body:**
```json
{
  "team_id": "integer",
  "password": "string"
}
```

**Response:**
```json
{
  "status": "200",
  "data": {
    "league_id": "integer",
    "team_id": "integer"
  },
  "message": "Team successfully joined the league"
}
```

**Expected Error:**
```json
{
  "status": "400 Bad Request",
  "message": "League has already reached the maximum number of teams or team is already registered."
}
```

```json
{
  "status": "403 Forbidden",
  "message": "Incorrect password for private league."
}
```

```json
{
  "status": "404 Not Found",
  "message": "League or team not found."
}
```

#### 3.5.18 Leave League
**DELETE** `/leagues/{league_id}/teams/{team_id}`

**Response:**
```json
{
  "status": "200",
  "message": "Team successfully removed from the league."
}
```

**Expected Errors:**
```json
{
  "status": "403 Forbidden",
  "message": "User does not own this team."
}
```

```json
{
  "status": "409 Conflict",
  "message": "Cannot leave a league that has already started."
}
```

### Behavior Management Endpoints

#### 3.5.19 Create Behavior
**POST** `/users/{user_id}/behaviors`

**Request Body:**
```json
{
  "name": "string",
  "code": "string"
}
```

**Response:**
```json
{
  "status": "201",
  "data": {
    "behavior_id": "integer",
    "name": "string",
  },
  "message": "Behavior created successfully."
}
```

**Expected Error:**
```json
{
  "status": "400 Bad Request",
  "message": "Syntax error in provided Python code."
}
```

#### 3.5.20 List Behaviors
**GET** `/users/{user_id}/behaviors`

**Response:**
```json
{
  "status": "200",
  "data": [
    {
      "name": "string"
    }
  ],
  "message": "Behaviors listed successfully."
}
```

#### 3.5.21 Get Behavior
**GET** `/users/{user_id}/behaviors/{behavior_id}`

**Response:**
```json
{ 
  "status": "200",
  "data": {
    "behavior_id": "integer",
    "name": "string",
    "code": "string"
  },
  "message": "Behavior retrieved successfully."
}
```

**Expected Error:**
```json
{
  "status": "404 Not Found",
  "message": "Behavior not found."
}
```

#### 3.5.22 Delete Behavior
**DELETE** `/users/{user_id}/behaviors/{behavior_id}`

**Response:**
```json
{
  "status": "200",
  "message": "Behavior deleted successfully."
}
```

**Expected Error:**
```json
{
  "status": "409 Conflict",
  "message": "Cannot delete behavior while assigned to an active player."
}
```

### Match Management Endpoints

#### 3.5.23 Schedule Match
**POST** `/leagues/{league_id}/matches/schedule`

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
  "status": "201",
  "data": {
    "match_id": "integer",
    "league_id": "integer",
    "home_team": "string",
    "away_team": "string",
    "scheduled_at": "datetime"
  },
  "message": "Match scheduled successfully."
}
```

**Expected Error:**
```json
{
  "status": "400 Bad Request",
  "message": "Schedule conflict or teams do not belong to this league."
}
```

#### 3.5.24 Start Match Simulacion
**POST** `/leagues/{league_id}/matches/match_id/start`

**Response:**
```json
{
  "status": "200",
  "data": {
    "match_id": "integer",
    "status": "in_progress",
    "started_at": "datetime"
  },
  "message": "Match simulation started successfully."
}
```

**Expected Errors:**
```json
{
  "status": "400 Bad Request",
  "message": "Match has already started or is finished."
}
```

```json
{
  "status": "403 Frobidden",
  "message": "User does not have permission to start this match."
}
```

```json
{
  "status": "404 Not Found",
  "message": "Match not found."
}
```

#### 3.5.25 Get Match
**GET** `/leagues/{league_id}/matches/{match_id}`

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
  "message": "Match retrieved successfully."
}
```

**Expected Error:**
```json
{
  "status": "404 Not Found",
  "message": "Match not found."
}
```

## 4. Real-Time Communication (WebSockets)

To enable live updates during matches, the system supports WebSocket connections in addition to REST endpoints. All WebSocket connections must include a valid JWT token in the `Authorization` header as `Bearer <token>`, matching the standard REST authentication method defined in Section **3.2**.

### 4.1 WebSocket Endpoint
- **Endpoint:** `/matches/{match_id}/websocket`
- **Protocol:** WebSocket (RFC 6455)
- **Base Domain:** The connection path is relative to the application's domain (e.g., `wss://api.futbot.com/v1/matches/{match_id}/websocket`).

### 4.2 Server-to-Client Messages
The server broadcasts events to all connected clients (spectators/coaches) during a match.

| Event Type | Description | Data Example |
|-------------|-------------|--------------|
| `match_update` | General match state update (score, time, quarter) | ```json {"type": "match_update", "data": {"score": "1-0", "time": "15:00", "quarter": 1}}``` |
| `match_event` | A specific event occurred (goal, behavior switches, player switches) | ```json {"type": "match_event", "data": {"event": "goal", "player": "Player A", "minute": 12}}``` |
| `substitution_event` | A substitution was made | ```json {"type": "substitution_event", "data": {"player_out": "Player X", "player_in": "Player Y"}}``` |

### 4.3 Client-to-Server Messages
Clients can send specific commands during a match.

| Event Type | Description | Data Example |
|-------------|-------------|--------------|
| `substitution_request` | Request to perform a substitution | ```json {"type": "substitution_request", "player_out_id": 1, "player_in_id": 2}``` |

---

## 5. Standard HTTP Status Codes

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