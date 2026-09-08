```plantuml
@startuml
title "Football game" - DFD Level 0

rectangle "Football game" as R1
rectangle "User (Client)" as U1
rectangle "Database (Data Store)" as D1

' Arrows with annotations added using the colon syntax
U1 --> R1 : User Inputs / Commands
R1 --> D1 : Read / Write Game Data
D1 --> R1 : Stored Data Response
U1 --> R1 : User Credentials / Profile Data
R1 --> U1 : Game State / UI Updates
@enduml
```


```plantuml
title DFD Level 1 - System Overview \n Core Functions

' External Entity
rectangle "User" as User

' Processes (Functions)

usecase "1.0\nUser/Profile Managment" as P1
usecase "2.0\nTeam Management" as P2
usecase "3.0\nLeague and Matches Engine" as P3
usecase "4.0\nBots and Behaviors Engine" as P4

' Data Store 
database "D1: User DB" as DB1
database "D2: Teams &\n Players DB" as DB2
database "D3: Matches DB" as DB3
database "D4: Bots DB" as DB4

User --> P1 : User Credentials\n(e.g., Emails, \nPasswords, Usernames)
P1 <--> DB1 : Read/Write User Data

User --> P2 : Team Data\n(Team names, PACSS, \nPlayers, Team-specific stats)
P2 <--> DB2 : Read/Write Teams \n& Players Data

User --> P3 : League Data\n(League names, Leaderboards, \nFixtures, Matches)
P3 <--> DB3 : Read/Write Leagues \n& Matches Data

User --> P4 : Bot Data\n(Behaviors, Python Scripts, \nPython Syntax Verifier) 
P4 <--> DB4 : Read/Write Bot \n& Behavior Data

P1 --> DB1
P2 --> DB2
P3 --> DB3
P4 --> DB4
```

```plantuml
title "DFD Level 2 - User/Profile Management"

rectangle "User" as User
usecase "1.1\nUser Registration" as UC1
usecase "1.2\nUser Login" as UC2
usecase "1.3\nProfile Edition" as UC3
usecase "1.4\nUser Logout" as UC4

database "D1: Users DB" as DB

User --> UC1 : Unregistered User Data\n(Username, Email, Password...)
User --> UC2 : User Login Credentials\n(Email, Password)
User --> UC3 : New User Credentials\n (Optional Email, Optional Password,...)
User --> UC4 : Log Out Request
UC1 --> DB : New User Record
UC2 --> DB : Updated Login timestamp
UC3 --> DB : Updated User records
UC4 --> DB : Token Session Invalidation Request
```

```plantuml
@startuml
title DFD Level 2 - Process 1.1 User Registration

rectangle "User" as EntityUser

' Level 2 Sub-Processes
usecase "1.1.1\nValidate Inputs" as P1_1
usecase "1.1.2\nCheck Email\nUniqueness" as P1_2
usecase "1.1.3\nHash Password &\nSave User" as P1_3

database "D1: Users DB" as DB

' Execution Pipeline
EntityUser --> P1_1 : Raw Registration Form Data
P1_1 --> EntityUser : Formatting Errors \n(Invalid Email/Password format)

P1_1 --> P1_2 : Formatted Inputs
P1_2 --> DB : Query Email Record
DB --> P1_2 : Existing Account Data
P1_2 --> EntityUser : Error: Email \nAlready Taken

P1_2 --> P1_3 : Unique & Validated Data
P1_3 --> DB : Write Encrypted User Record
P1_3 --> EntityUser : Registration Success Response
@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 1.2 User Login

' External Entity
rectangle "User" as EntityUser

' Sub-Processes (Level 2)
usecase "1.2.1\nValidate Field\nFormats" as P1
usecase "1.2.2\nAuthenticate\nCredentials" as P2
usecase "1.2.3\nGenerate User\nSession" as P3

' Data Store
database "D1: Users DB" as DB

' Data Flows
EntityUser --> P1 : Login Credentials\n(Email, Raw Password)
P1 --> EntityUser : Field Validation Errors\n(e.g., Empty fields, bad email format)

P1 --> P2 : Formatted Credentials

' Database Interaction Loop
P2 --> DB : Query User Record by Email
DB --> P2 : Encrypted Hash & User Data
P2 --> EntityUser : Error: Invalid Email or Password

' Success Pathway
P2 --> P3 : Validated User Identity
P3 --> DB : Update Last Login Timestamp
P3 --> EntityUser : Auth Response

@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 1.3 Edit User Profile

rectangle "User" as Us
database "D1: Users DB" as DB1

usecase "1.3.1\nValidate Field\nFormats & Matching" as US1
usecase "1.3.2\nVerify Current\nPassword" as US2
usecase "1.3.3\nHash Password\n& Update User" as US3

' Step 1: Format Checks
Us --> US1 : Profile Update Inputs\n(Name, Email, Avatar, Passwords)
US1 --> Us : Error: Invalid Email Format\nor New/Repeat Password Mismatch
US1 --> US2 : Formatted Profile Data\n& Credentials Check Request

' Step 2: Authentication Check
US2 --> DB1 : Query Existing User Hash
DB1 --> US2 : Current Password Hash
US2 --> Us : Error: Incorrect Current Password
US2 --> US3 : Validated Profile Updates\n& New Password Data

' Step 3: Hash and Save
US3 --> DB1 : Update User Record
US3 --> Us : Profile Updated Success Response

@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 1.4 Log Out

rectangle "User" as EntityUser

usecase "1.4.1\nValidate Active\nSession" as P1
usecase "1.4.2\nConfirm Logout\nIntent" as P2
usecase "1.4.3\nInvalidate Session &\nTerminate Connections" as P3

database "D1: Users DB" as DB

' Data Flows
EntityUser --> P1 : Logout Action\n(from any page)
P1 --> DB : Verify Session/Token Status
DB --> P1 : Session Status

P1 --> P2 : Active Session Confirmed
P2 --> EntityUser : Confirmation Prompt\n("Are you sure you want to log out?")
EntityUser --> P2 : Confirm / Cancel

P2 --> EntityUser : Cancel: No Changes\n(Remains Logged In)

P2 --> P3 : Confirmed Logout Request
P3 --> DB : Invalidate Session Token
P3 --> EntityUser : Close WebSocket Connection\n(Match Updates)
P3 --> EntityUser : Clear Client-Side Data\n(Local Storage, Session Vars)
P3 --> EntityUser : Logout Success &\nRedirect to Login Page

@enduml

```

---
# Player Management
```plantuml
@startuml
title DFD Level 2 - Process 2.1 Create Player

rectangle "User" as Us
database "D2: Teams & Players DB" as DB2

usecase "2.1.1\nValidate Name &\nPACSS Rules" as P1
usecase "2.1.2\nSave Player &\nAssign Team" as P2

' Step 1: Input & PACSS Validation
Us --> P1 : Player Data\n(Name, PACSS Attributes)
P1 --> Us : Error: Invalid PACSS Allocation\n(Total Must Equal 300 Points)
P1 --> Us : Error: Attribute Below Minimum\n(Min 1 Point Per Attribute)
P1 --> P2 : Validated Player Profile

' Step 2: Database Persistence & Output
P2 --> DB2 : Write New Player Record
P2 --> Us : Player Creation Success Response

@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 2.2 Update Player

rectangle "User" as Us
database "D2: Teams & Players DB" as DB2

usecase "2.2.1\nValidate Match\nEligibility" as P1
usecase "2.2.2\nValidate PACSS\nRe-allocation" as P2
usecase "2.2.3\nUpdate Player\nRecord" as P3

' Step 1: Match Lock Check
Us --> P1 : Update Request\n(Player ID, Updated Name, PACSS)
P1 --> DB2 : Query Active\n Match Status
DB2 --> P1 : Player Match\n Status
P1 --> Us : Error: Cannot Edit Player\n(Currently In Active Match)
P1 --> P2 : Approved Update\n Payload

' Step 2: PACSS Validation
P2 --> Us : Error: Invalid PACSS Allocation\n(Total Must Equal\n 300 Points)
P2 --> P3 : Validated Updated\n Attributes

' Step 3: Database Persistence & Output
P3 --> DB2 : Update Player Entry \n& Team Mapping
P3 --> Us : Player Update \nSuccess Response

@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 2.3 Delete Player

rectangle "User" as Us
database "D2: Teams & Players DB" as DB2

usecase "2.3.1\nValidate Deletion\nEligibility" as US1
usecase "2.3.2\nExecute Player\n& Roster Removal" as US2

' Step 1: Eligibility Check
Us --> US1 : Delete Player Request \n(Player ID)
US1 --> DB2 : Query Active League\n & Team Status
DB2 --> US1 : Player Assignment\n Records
US1 --> Us : Error: Cannot Delete \n(Active League)
US1 --> Us : Warning: Player in Team \n(Requires Proceed Confirmation)

' Step 2: Execution & Cascade Update
Us --> US2 : Confirmed Deletion\n Request
US2 --> DB2 : Delete Player\n Record
US2 --> DB2 : Cascade Update\n Team Roster
US2 --> Us : Player Deleted\n Success Response

@enduml
```
---
# Team Management

```plantuml
@startuml
title DFD Level 2 - Process 3.1 Create Team

rectangle "User" as Us
database "D2: Teams & Players DB" as DB2

usecase "3.1.1\nValidate Roster\nCount & Inputs" as P1
usecase "3.1.2\nVerify Player\nAvailability" as P2
usecase "3.1.3\nSave Team &\nAssign Players" as P3

' Step 1: Input & Size Validation
Us --> P1 : Team Creation Data\n(Team Name, 6+ Players)
P1 --> Us : Error: Insufficient Players\n(Minimum 3+3 Required)
P1 --> P2 : Formatted Team Data\n& Valid Roster Count

' Step 2: Database Assignment Check
P2 --> DB2 : Query Player\n Assignment Status
DB2 --> P2 : Current Player/Team Mappings
P2 --> Us : Error: Player(s) Already Assigned\nTo Another Team

P2 --> P3 : Fully Validated\n Team Payload

' Step 3: Database Persistence
P3 --> DB2 : Write New\n Team Record
P3 --> DB2 : Map Players to\n Team
P3 --> Us : Team Creation\n Success Response

@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 3.2 Manage Team Players

rectangle "User" as Us
database "D2: Teams &\n Players DB" as DB2

usecase "3.2.1\nValidate Roster\nAction" as P1
usecase "3.2.2\nUpdate Roster\nRelationship" as P2

' Step 1: Input & Roster Validation
Us --> P1 : Roster Action\n(Team, Player,\n Add/Remove)
P1 --> DB2 : Query Current\n Team Roster
DB2 --> P1 : Existing Roster\n List

P1 --> Us : Error: Player Already\n in Team (Add Action)
P1 --> Us : Error: Player Not\n in Team (Remove Action)

P1 --> P2 : Validated Roster\n Update Payload

' Step 2: Database Persistence
P2 --> DB2 : Add/Remove Player-Team\n Mapping
P2 --> Us : Roster Update\n Success Response

@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 3.3 Delete Team

rectangle "User" as Us
database "D2: Teams & Players DB" as DB2
database "D3: Leagues & Matches DB" as DB3

usecase "3.3.1\nValidate Deletion\nEligibility" as P1
usecase "3.3.2\nExecute Team\nDeletion" as P2

' Step 1: Eligibility Check
Us --> P1 : Delete Team Request\n(Team ID)
P1 --> DB3 : Query Team's League\nMembership Status
DB3 --> P1 : League Registration Data
P1 --> DB3 : Query Team's Friendly\nMatch Status
DB3 --> P1 : Friendly Match Status

P1 --> Us : Error: Cannot Delete Team\n(Registered in League {league name})
P1 --> Us : Error: Cannot Delete Team\n(Currently Playing Friendly Match)

P1 --> P2 : Validated Deletion Request

' Step 2: Execution
P2 --> DB2 : Delete Team Record
P2 --> Us : Team Deleted\nSuccess Response

@enduml
```

--- 

# Leagues Management

```plantuml
@startuml
title DFD Level 2 - Process 4.1 Create League

rectangle "User" as Us
database "D3: Leagues & Matches DB" as DB3
database "D2: Teams & Players DB" as DB2

usecase "4.1.1\nValidate League Name\n& Format" as P1
usecase "4.1.2\nValidate Team Count\n(Min/Max)" as P2
usecase "4.1.3\nSave League &\nAssign Teams" as P3

' Step 1: Name Validation
Us --> P1 : League Data\n(Name, Type, Password,\nMin/Max Teams, Duration, Start Date)
P1 --> DB3 : Query Existing\nLeague Names
DB3 --> P1 : Name Match Result
P1 --> Us : Error: Invalid League Name\n(Empty or Duplicate)

' Step 2: Team Count Validation
P1 --> P2 : Formatted League Data
P2 --> Us : Error: Insufficient Teams\n(Minimum 3 Required)
P2 --> Us : Error: Min Teams\nGreater Than Max Teams
P2 --> P3 : Validated League Payload\n& Team Count

' Step 3: Persistence
P3 --> DB3 : Write New League Record\n(Configs, Rules, Type)
P3 --> DB2 : Verify Selected Teams\nExist / Owned by User
DB2 --> P3 : Team Verification Result
P3 --> DB3 : Assign Selected Teams\nto League
P3 --> Us : League Creation\nSuccess Response
P3 --> Us : Error: Teams Do Not Exist\n or Not Owned by User

@enduml
```

```plantuml
title DFD Level 2 - Process 4.2 Schedule Matches

rectangle "User" as Us
database "D3: Leagues & Matches DB" as DB3

usecase "4.2.1\nValidate Teams Belong\nto League" as P1
usecase "4.2.2\nValidate Schedule\n(No Overlap)" as P2
usecase "4.2.3\nSave Scheduled\nMatch" as P3

' Step 1: Team Membership Validation
Us --> P1 : Match Data\n(League ID, Home Team,\nAway Team, Scheduled Date/Time)
P1 --> DB3 : Query League's\nRegistered Teams
DB3 --> P1 : Registered Team List
P1 --> Us : Error: Teams Not in\nSame League

' Step 2: Overlap Validation
P1 --> P2 : Verified Team Membership
P2 --> DB3 : Query Existing Matches\nfor Both Teams
DB3 --> P2 : Existing Match Schedule
P2 --> Us : Error: Schedule Conflict\n(Overlapping Match)

' Step 3: Persistence
P2 --> P3 : Validated Match Payload
P3 --> DB3 : Write New Scheduled\nMatch Record
P3 --> Us : Match Scheduled\nSuccess Response
```


```plantuml
@startuml
title DFD Level 2 - Process 4.4 Join Private League

rectangle "User" as Us
database "D3: Leagues & Matches DB" as DB3
database "D2: Teams & Players DB" as DB2
database "D4: Bots & Behaviors DB" as DB4

usecase "4.4.1\nLocate & Authenticate\nPrivate League" as P1
usecase "4.4.2\nValidate Team\nEligibility" as P2
usecase "4.4.3\nAssign Player\nBehaviors" as P3
usecase "4.4.4\nValidate Capacity &\nRegister Team" as P4

' Step 1: Locate & Password Check
Us --> P1 : League Name & Password\n(from "Join Private League" modal)
P1 --> DB3 : Query Private League\nby Name
DB3 --> P1 : League Record\n& Stored Password
P1 --> Us : Error: League Not Found
P1 --> Us : Error: Invalid Password

' Step 2: Team Selection & Validation
P1 --> P2 : Authenticated League\nAccess Granted
P2 --> DB2 : Query User's Teams\n& League/Roster Status
DB2 --> P2 : Team Eligibility Data
P2 --> Us : Team List\n(Ineligible Teams Grayed Out)
Us --> P2 : Selected Team

' Step 3: Behavior Assignment
P2 --> P3 : Validated Team\n& Player Roster
P3 --> DB4 : Query Available\nBehaviors (incl. default_behavior)
DB4 --> P3 : Behavior List
Us --> P3 : Behavior Assignments\n(Per Player)

' Step 4: Final Join & Persistence
Us --> P4 : Final "Join" Click
P4 --> DB3 : Query Current League\nParticipant Count
DB3 --> P4 : Capacity Status
P4 --> Us : Error: League at\nMaximum Participant Limit
P4 --> DB3 : Register Team in League
P4 --> DB2 : Save Player Behavior\nAssignments
P4 --> Us : Join Success &\nRedirect to League Dashboard

@enduml
```
```plantuml
@startuml
title DFD Level 2 - Process 4.5 Leave League

rectangle "User" as Us
database "D3: Leagues & Matches DB" as DB3

usecase "4.5.1\nConfirm Leave\nIntent" as P1
usecase "4.5.2\nValidate League\nHas Not Started" as P2
usecase "4.5.3\nRemove Team &\nRecalculate Fixtures" as P3
usecase "4.5.4\nBroadcast Update &\nNotify User" as P4

' Step 1: Confirmation
Us --> P1 : "Leave League" Click\n(on League Dashboard)
P1 --> Us : Confirmation Prompt\n("Team will be available immediately")
Us --> P1 : Confirm / Cancel
P1 --> Us : Cancel: No Changes\n(Remains in League)

' Step 2: Eligibility Validation
P1 --> P2 : Confirmed Leave Request
P2 --> DB3 : Query League Match\n/ Start Status
DB3 --> P2 : League Status\n(Started? Active Matches?)
P2 --> Us : Error: Cannot Leave\n(League Already Started)

' Step 3: Removal & Recalculation
P2 --> P3 : Validated Leave Request
P3 --> DB3 : Remove Team-League\nMapping
P3 --> DB3 : Preserve Team's\nScore & Stats
P3 --> DB3 : Remove Team's Remaining\nScheduled Matches
P3 --> DB3 : Adjust League\nStandings

' Step 4: Broadcast & Redirect
P3 --> P4 : Fixtures Recalculated
P4 --> Us : WebSocket Broadcast\n(To All League Participants)
P4 --> Us : Leave Success &\nRedirect to Main Dashboard\n(Team Now Available)

@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 4.6 List User Leagues

rectangle "User" as Us
database "D3: Leagues & Matches DB" as DB3

usecase "4.6.1\nQuery User's\nLeagues" as P1
usecase "4.6.2\nFormat &\nDisplay List" as P2

' Step 1: Query
Us --> P1 : Navigate to\n"My Leagues"
P1 --> DB3 : Query Leagues Where\nUser is Creator or Team Participant
DB3 --> P1 : League Records
P1 --> Us : Error: Data Load Failed\n/ Session Expired

' Step 2: Display
P1 --> P2 : Retrieved League Records
P2 --> Us : League List\n(Name, ID, Status, Enrolled Team Count)
P2 --> Us : Empty State: "Not Enrolled\nin Any League" + Shortcuts

@enduml
```

# Behavior Management

```plantuml
@startuml
title DFD Level 2 - Process 5.1 Create Behavior

rectangle "User" as Us
database "D4: Bots & Behaviors DB" as DB4

usecase "5.1.1\nValidate Python\nSyntax" as P1
usecase "5.1.2\nSave Behavior\nRecord" as P3

' Step 1: Syntax Validation
Us --> P1 : Behavior Data\n(Name, Python Script, Description)
P1 --> Us : Error: Invalid Python Syntax\n(Correction Prompt)

' Step 2: Syntactic Validation
P1 --> P3 : Syntactically Valid Script

' Step 3: Persistence
P3 --> DB4 : Write New Behavior Record
P3 --> Us : Behavior Creation\nSuccess Response

@enduml
```

# Match Management

```plantuml
@startuml
title DFD Level 2 - Process 6.1 Execute Match

rectangle "Scheduler\n(System Trigger)" as Sched
database "D3: Leagues & Matches DB" as DB3
database "D2: Teams & Players DB" as DB2
database "D4: Bots & Behaviors DB" as DB4
rectangle "Spectators /\nParticipants" as Spec

usecase "6.1.1\nIdentify Scheduled\nMatch & Load Teams" as P1
usecase "6.1.2\nLoad Player Behaviors\n(Fallback to Default)" as P2
usecase "6.1.3\nRun Match Simulation\n(Live WebSocket Updates)" as P3
usecase "6.1.4\nStore Results &\nUpdate Standings" as P4

' Step 1: Identify & Load
Sched --> P1 : Match Start Trigger\n(scheduled_at Reached)
P1 --> DB3 : Query Scheduled Match\n& Team Compositions
DB3 --> P1 : Match & Team Data

' Step 2: Behavior Loading
P1 --> P2 : Team Rosters
P2 --> DB2 : Query Player Behavior\nAssignments
DB2 --> P2 : Assigned Behaviors\n(or None)
P2 --> DB4 : Fallback: Load\ndefault_behavior
DB4 --> P2 : Default Behavior Script

' Step 3: Simulation
P2 --> P3 : Initialized Match\nEnvironment
P3 --> Spec : WebSocket: match_update\n(Score, Time, Quarter)
P3 --> Spec : WebSocket: match_event\n(Goals, Behavior/Player Switches)
P3 --> P4 : Simulation Complete\n(or Execution Error)

' Step 4: Persistence & Final Broadcast
P4 --> DB3 : Write Match Results\n(Scores, Status = Finished)
P4 --> DB3 : Update League Standings
P4 --> Spec : WebSocket: Final\nResults Broadcast

@enduml
```
