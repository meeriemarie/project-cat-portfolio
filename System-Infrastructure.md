## Overview
The game is built with modular, component-based architecture in Unity. Each gameplay system (player control, interaction, AI, scoring, UI, etc.) is represented through dedicated C# classes and MonoBehaviours. Below is the class design structure focusing on relationships, not detailed implementation.

---

## Class Relationship Diagram
```mermaid
classDiagram

class GameManager {
  +Handle game states
}

class ScoreManager {
  +Track points
}

class AudioManager {
  +Play SFX & BGM
}

class UIManager {
  +Update HUD
}

GameManager --> ScoreManager
GameManager --> AudioManager
GameManager --> UIManager
```

```mermaid
classDiagram

class RoomManager {
  +Manage room layout
  +Trigger unlocks
}

class Room {
  +Room type
}

class ProceduralSpawner {
  +Spawn interactables per room type
}

RoomManager --> Room
RoomManager --> ProceduralSpawner 
```

```mermaid
classDiagram

class PlayerController {
  +Handle movement
  +Interact with InteractableObject
  +Trigger audio & animation
  +Inform ScoreManager
}

class CameraController {
  +Follow PlayerController
}
```

```mermaid
classDiagram

class InteractableObject {
  +React to interaction
  +Assign points to ScoreManager
  +Play sound/animation
}

class ChaserBase {
  <<abstract>>
  +Patrol & detect
}

class HumanChaser {
  +Slow but smart
}

class DogChaser {
  +Fast, can dash
}

ChaserBase <|-- HumanChaser
ChaserBase <|-- DogChaser
```