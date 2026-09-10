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
**GET** `/users`
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

#### 3.5.10 List League Teams
**GET** `/leagues/{league_id}/teams`

**Response:**
```json
{
  "status": "200",
  "data": [
    {
      "team_id": "integer",
      "name": "string"
    }
  ],
  "message": "League teams listed successfully."
}
```

**Expected Errors:**
```json
{
  "status": "404 Not Found",
  "message": "League not found."
}
```

#### 3.5.11 Get Team
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

#### 3.5.12 Assign Behavior to Player
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



#### 3.5.13 Delete Team
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
  "message": "Cannot delete team while register in an active league or playing friendly match."
}
```

### League Management Endpoints

#### 3.5.14 Create League
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

```json
{
    "status": "409 Conflict",
    "message": "League name in use." 
}
```

#### 3.5.15 List All Leagues
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

#### 3.5.16 List User Leagues
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
  "message": "Leagues retrieved successfully."
}
```

#### 3.5.17 Get User League
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
  "message": "League retrieved successfully. Notification sent to teams."
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
#### 3.5.18 Join League
**POST** `/leagues/{league_id}/join`

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

#### 3.5.19 Leave League
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

#### 3.5.20 Create Behavior
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
    "name": "string"
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

#### 3.5.21 List Behaviors
**GET** `/users/{user_id}/behaviors`

**Response:**
```json
{
  "status": "200",
  "data": [
    {
      "behavior_id": "integer",
      "name": "string"
    }
  ],
  "message": "Behaviors listed successfully."
}
```

#### 3.5.22 Get Behavior
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

#### 3.5.23 Edit Behavior
**PUT** `/users/{user_id/behaviors/{behavior_id}`

**Request Body**
```json
{
  "name": "string",
  "code": "string"
}
```

**Response:**
```json
{
  "status": "200",
  "data": {
    "name": "string",
    "code": "string"
  },
  "message": "Behavior change successfully."
}
```

**Expected Errors:**

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

#### 3.5.24 Delete Behavior
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

#### 3.5.25 Schedule Match
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
#### 3.5.26 Send Friendly Match Request
**POST** `/users/{user_id}/friendly-requests`

**Request Body:**
```json
{
  "receiver_user_id": "integer",
  "sender_team_id": "integer",
  "reveiver_team_id": "integer"
}
```

**Response:**
```json
{
  "status": "201",
  "data": {
    "request_id": "integer",
    "status": "pending"
  },
  "message": "Friendly match request sent successfully."
}
```

**Expected Errors:**
```json
{
  "status": "400 Bad Request",
  "message": "Cannot send a request to yourself."
}
```

```json
{
  "status": "404 Not Found",
  "message": "Receiver user or team does not exist."
}
```

#### 3.5.27 Respond to Friendly Request
**PATCH** `/users/{user_id}/friendly-requests/{request_id}`

**Request Body:**
```json
{
  "status": "accepted"    // accepted or rejected
}
```
**Response: Accepted**
```json
{
  "status": "200",
  "data": {
    "request_id": "integer",
    "status": "accepted",
    "match_id": "integer"
  },
  "message": "Request accepted. Match created with starting 3 players."
}
```

**Response: Rejected**
```json
{
  "status": "200",
  "data": {
    "request_id": "integer",
    "status": "rejected"
  },
  "message": "Request rejected. Notification sent to sender."
}
```

**Expected Errors:**
```json
{
  "status": "403 Forbidden",
  "message": "User is not the receiver of request."
}
```

```json
{
  "status": "404 Not Found",
  "message": "Request not found."
}
```

#### 3.5.28 Start Friendly Match Simulacion
**POST** `/matches/match_id/start`

**Response:**
```json
{
  "status": "200",
  "data": {
    "match_id": "integer",
    "status": "in_progress"
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
  "message": "User is not a participant in this friendly match."
}
```

```json
{
  "status": "404 Not Found",
  "message": "Match not found."
}
```

#### 3.5.29 Get Match
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
- **Authentication:** A valid JWT token must be provided via `Authorization` header as `Bearer <token>` during the connection handshake.

### 4.2 Server-to-Client Messages
The server broadcasts events to all connected clients (spectators/coaches) during a match.

| Event Type           | Description                                                          | Data Example                                                                                           |
| -------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `match_update`       | General match state update (score, time, quarter)                    | ```json {"type": "match_update", "data": {"score": "1-0", "time": "15:00", "quarter": 1}}```           |
| `match_event`        | A specific event occurred (goal, behavior switches, player switches) | ```json {"type": "match_event", "data": {"event": "goal", "player": "Player A", "minute": 12}}```      |
| `substitution_event` | A substitution was made                                              | ```json {"type": "substitution_event", "data": {"player_out": "Player X", "player_in": "Player Y"}}``` |


### 4.3 Client-to-Server Messages
Clients can send specific commands during a match.

| Event Type                | Description                                                        | Data Example                                                                                    |
| ------------------------- | ------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------- |
| `substitution_request`    | Request to perform a substitution                                  | ```json {"type": "substitution_request", "player_out_id": 1, "player_in_id": 2}```              |
| `behavior_change_request` | Request to modify a player's tactics/behavior strategy on the fly. | ```json {"type": "behavior_change_request", "data": {"player_id": 10, "new_behavior_id": 3}}``` |

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
|`409 Conflict`| Conflict with other process.|
| `500 Internal Server Error` | Server-side error occurred. |


## 6. Behaviors API

### 6.1 Overview
This library provides the necessary tools and documentation for users to design and program their players' behavior logic using the Python programming language. The game system will process the decisions made in each turn and execute the behaviors, automatically resolving their success and physical performance through the player's PACSS system attributes (POWER, AGILITY, CONTROL, SPEED, and STRENGTH).

### 6.2 Entry Point: The `jugar_turno` Method
For the simulator system to communicate with the user's code, it is mandatory that the Python script includes a main function named `jugar_turno(estado_partida)`. This method acts as the core of the players' actions and is the only place where their behavior logic can be developed.

*   **Execution Frequency:** The game engine does not call this function just once. It invokes it repeatedly several times per second during each "tick" or match update cycle to constantly evaluate the situation.
*   **Obtaining the Match State:** The method obtains environmental information exclusively through parameters. The server engine injects the `estado_partida` object as an argument when invoking `jugar_turno`. No external primitive calls or network queries are required from the user to receive this update, as data delivery is a system-managed process.
*   **The Main Function Parameter:** Every time the server calls the `jugar_turno` function, it injects an object named `estado_partida`. This object acts as the player's senses and contains read-only information about the current instant of the match, such as:
    *   **Ball data:** Its current spatial coordinates and, if someone possesses it at that moment, the ID of that player.
    *   **Player data:** Precise coordinates of both teammates and rivals, which will be used to calculate distances and make behavioral decisions.
    *   **Match data:** General information about the environment, such as the remaining time and the current score.
*   **Continuity Rule:** Players will only change their current behavior if the script explicitly executes a new primitive during the turn. In the event that the behavior finishes its action and the system receives no primitive change, it will not stop the bot, but rather maintain it continuously performing the behavior it was assigned in the previous tick.

### 6.3 PRIMITIVES CATALOG

#### 6.3.1 `moverse_hacia`
**METHOD** `moverse_hacia(x: float, y: float, porcentaje_velocidad: int) -> None`

*   **Description:** Orders the player to move towards the (x, y) coordinates provided as parameters, using a speed percentage consistent with their speed attribute.
*   **State Impact:** Progressively updates the player's own coordinates in subsequent ticks.
*   **Evaluated Attributes:**
    *   **SPEED:** Determines the player's maximum movement speed and acceleration capacity during the trajectory.

#### 6.3.2 `mantener_posicion`
**METHOD** `mantener_posicion(x: float, y: float, radio: float) -> None`

*   **Description:** Keeps the player in a specified zone of the field using the radius to establish future defensive strategies or wait for a teammate's pass.
*   **State Impact:** Sets the input (x, y) coordinates as the central reference point and delimits the area using the specified radius. The bot will move to that zone and remain in motion within that limit until the ball or a rival invades it.
*   **Evaluated Attributes:**
    *   **SPEED:** Used by the system to react to invasions by rivals or the ball within its established radius.

#### 6.3.3 `marcar_pelota`
**METHOD** `marcar_pelota() -> None`

*   **Description:** Orders the player to attempt to tackle the ball from the rival player who currently possesses it.
*   **State Impact:** If the action is successful, the rival player will lose possession of the ball.
*   **Evaluated Attributes:**
    *   **CONTROL:** The engine first validates if the player is close enough to the rival according to the reach radius dictated by this attribute.
    *   **STRENGTH:** The success of the tackle is calculated through a direct dispute of this value against the opponent's strength.

#### 6.3.4 `patear_pelota`
**METHOD** `patear_pelota(x: float, y: float, porcentaje_fuerza: int) -> None`

*   **Description:** The player strikes the ball towards the (x, y) coordinates using a percentage of their maximum strength.
*   **State Impact:** Propels the ball with a specific speed and direction, and applies a temporary restriction on the player's ability to strike again.
*   **Evaluated Attributes:**
    *   **CONTROL:** The system first verifies if the distance between the player and the ball is less than or equal to the reach radius dictated by this attribute.
    *   **POWER:** Scales the input strength percentage to calculate the ball's exit speed.
    *   **AGILITY:** Determines the amount of penalty ticks the player will suffer before being able to shoot again.

#### 6.3.5 `pasar_pelota`
**METHOD** `pasar_pelota(id_jugador: int, porcentaje_fuerza: int) -> None`

*   **Description:** Calculates the current coordinates of the teammate indicated by their ID and executes a pass directed towards their position.
*   **State Impact:** Propels the ball with a specific speed and direction towards the receiver's location at the time of the pass and applies a temporary restriction on the player's ability to strike again.
*   **Evaluated Attributes:**
    *   **CONTROL:** Verifies if the distance between the player and the ball is valid to initiate the pass.
    *   **POWER:** Scales the input strength percentage to calculate the exit speed of the pass.
    *   **AGILITY:** Determines the amount of penalty ticks the player will suffer before being able to shoot again.