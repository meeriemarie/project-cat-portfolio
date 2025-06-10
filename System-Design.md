## Overview  
This system design outlines the functional relationships between the core characters (player cat, human chaser) and the evolving structure of the game world (scenes/rooms) as the player progresses. The design focuses on the flow of gameplay from room to room, conditions for progression, and interactions between player and NPC.

---

##  Characters

### 1. Cat (Player)
- Starts in the hallway.
- Can explore and interact with destructible objects.
- Progresses by destroying a set number of objects per room.
- Triggers room unlocks and human chaser spawn.

### 2. Human (Chaser #1)
- Only spawns once the player unlocks the bedroom.
- Actively chases the cat after spawning.
- If the cat is caught, the game resets to the beginning.
- Does not appear before the bedroom is unlocked so the player can familiarize themselves with the game.

### 3. Dog (Chaser #2)
- Only Spawns once the player unlocks more rooms (mid/late-game).
- Faster than the Human and operates on simpler, aggressive AI.
- If the cat is caught, the game resets to the beginning.
- Does not appear before mid/late-game milestone so the player can familiarize themselves with the Human (Chaser #1).
---

##  Scenes / Rooms

Each “scene” is technically part of one apartment-level scene, but functionally treated as separate zones due to locked doors.

| Room         | Entry Condition                            | Exit Condition / Unlock Trigger          |
|--------------|---------------------------------------------|------------------------------------------|
| **Hallway**      | Default starting location                   | Destroy objects to unlock Living Room    |
| **Living Room**  | Unlocked from the start                     | Destroy objects to unlock Bedroom        |
| **Bedroom**      | Unlock spawns human chaser                  | Destroy objects to unlock next room      |
| **Additional rooms** | Progressively unlocked via object destruction | Final score unlocks Apartment Exit      |
| **Exit**         | Only unlocked at final score threshold      | Player must reach this room to win       |

---

##  Gameplay Flow (Functional Logic)

```mermaid
graph TD
    Start(Hallway Spawn) --> AccessLivingRoom[Player Enters Living Room Freely]
    AccessLivingRoom --> DestroyPhase1[Destroy Objects in Living Room]
    DestroyPhase1 -->|Unlock| Bedroom
    Bedroom -->|Trigger| ChaserSpawn(Human Spawns)
    Bedroom --> BedroomDestruction[Destroy More Objects]
    BedroomDestruction -->|Unlock| MoreRooms
    MoreRooms -->|Trigger| 2ndChaserSpawn(Dog Spawns) 
    MoreRooms --> FinalDestruction[Destroy Enough to Reach Final Score]
    FinalDestruction --> ExitDoor[Exit Door Unlocks]
    ExitDoor --> Victory[Player Escapes]
    ChaserSpawn --> ChaseLogic(Human Chases Cat)
    2ndChaserSpawn --> 2ndChaseLogic(Dog Chases Cat)
    ChaseLogic -->|If caught| Start
    2ndChaseLogic -->|If caught| Start

