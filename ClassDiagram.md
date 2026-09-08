# CLASS DIAGRAM
```mermaid
classDiagram
    class User {
        +int id
        +String nickname
        +String email
        +String password
        +String avatar

        +register()
        +login()
        +manageProfile()
    }

    class Team {
        +int id
        +String name
        +List~Player~ startingPlayers
        +List~Player~ substitutePlayers
        
        +createTeam()
        +isLeagueReady()
        +joinLeague()
        +editTeam()
        +deleteTeam()
        +assignBehavior()
    }
    
    class Record {
        +int leaguesWon
        +int totalPoints
        +int matchesWon
        +int matchesLost
        +int matchesDrawn
        +List~Match~ matches
    }

    class Player {
        +int id
        +int shirtNumber
        +String name
        +int power
        +int agility
        +int control
        +int speed
        +int strength

        +updatePlayer()
        +deletePlayer()
        +validatePACSS()
    }

    class Behavior {
        +int id
        +String name
        +String pythonCode

        +createBehavior()
        +validateSyntax()
        +validateSecurity()
        +editBehavior()
        +deleteBehavior()
        +isBehaviorInUse()
    }

    class League {
        +int id
        +String name
        +bool isPrivate
        +String password
        +int minTeams
        +int maxTeams
        +int matchDuration
        +int startDate
        +List~Team~ registeredTeams

        +createLeague()
        +startLeague()
        +cancelLeague()
    }

    class FriendlyMatchRequest {
        +int id
        +String status
        
        +accept()
        +reject()
    }

    class StandingsTable {
        +List~Team~ registeredTeams
        +String order

        +update()
        +reset()
    }

    class GlobalRanking {
        +List~User~ users
        +String order

        +updateRanking()
    }

    class Match {
        +int id
        +bool inProgress 
        +MatchResult matchResult

        +startMatch()
        +processSubstitution()
        +endMatch()
    }

    class MatchResult {
        +int team1Goals
        +int team2Goals

        +getWinner()
        +isDraw()
    }

    User "1" -- "1" Team : owns
    User "1" --> "*" Behavior : creates
    User "*" --> "1" Match: observes
    User "1" --> "*" Player: creates
    
    User "1" --> "*" FriendlyMatchRequest: sends/receives
    FriendlyMatchRequest "1" --> "1" Match: generates

    Team "1" o-- "6" Player : consists of
    Team --> Record
    Team "*" --> "1" Behavior : assigns to player

    League "1" o-- "*" Team : registers
    League "1" *-- "1" StandingsTable : manages
    League "1" *-- "*" Match : schedules

    Match "*" --> "2" Team : compete

    Match --> MatchResult: has

    User "*" --> "1" GlobalRanking: composes
```

# Data Dictionary

### 1. Core Entities

| Entity | Field | Data Type | Description |
| :--- | :--- | :--- | :--- |
| **User** | `user_id` | Integer | Unique auto-generated identifier. |
| **User** | `username`, `email`, `password` | String | Unique access credentials. |
| **User** | `created_at` | Datetime | Date and time of registration. |
| **Player** | `player_id`, `team_id`, `shirt_numb` | Integer | Relational IDs and shirt number. |
| **Player** | `pacss_attributes` | Object | Integer numerical values for *power*, *agility*, *control*, *speed*, and *strength*. |
| **Team** | `starting_players` / `substitutes` | Array | List of objects linking `player_id` and `behavior_id`. |

---

### 2. Competition Entities

| Entity | Field | Data Type | Description |
| :--- | :--- | :--- | :--- |
| **League** | `is_private` | Boolean | Defines if a password is required for access. |
| **League** | `min_teams`, `max_teams`, `duration` | Integer | Numerical rules and capacity. |
| **Match** | `home_team_id`, `away_team_id` | Integer | Opposing teams. |
| **Match** | `scheduled_at` | Datetime | Scheduled date for automatic start. |
| **Friendly Match** | `status` | String | Status of the request (e.g., `"pending"`, `"accepted"`, `"rejected"`). |

---

### 3. Behavior Entities

| Entity | Field | Data Type | Description |
| :--- | :--- | :--- | :--- |
| **Behavior** | `behavior_id` | Integer | Tactic identifier. |
| **Behavior** | `name` | String | Descriptive name assigned by the user. |
| **Behavior** | `code` | String | Text block storing the Python script. |
