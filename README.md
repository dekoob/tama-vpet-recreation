# 🐾 Tamagotchi Project - A Virtual Companion

Tamagotchi Project is a Java-based virtual pet application where users care for a digital companion by feeding, playing, and resting it. The pet responds to your actions and evolves through different emotional and physical states, providing a fun way to demonstrate object-oriented design, design patterns, and Java concurrency.


---

## Features

**Overview:**
- Interactive pet simulation with real-time updates
- Tracks hunger, energy, and mood
- Pet state changes dynamically (Happy, Hungry, Sick)
- Uses key Object-Oriented Design Principles
- Implements 4 major Design Patterns
- Utilizes game loop using `Runnable` and `Thread` for time-based stat changes

**1. Interactive Pet Care Simulation**
- The user interacts with the pet using the keyboard
- Pressing specific keys `A, F, R, P` triggers pet actions like feeding, playing, or resting.

**2. Dynamic State Transition Based on Pet's Stats**
- The pet automatically changes behavior when its hunger, energy, or mood reaches certain thresholds.
- For example: if energy < 30 → pet becomes tired or sick.

**3. Real-Time Status Monitoring**
- The pet's hunger, energy, and mood are updated in real time.
- Observers log changes immediately for visual or data feedback.

**4. Customize Appearance**
- Users can decorate the pet's room or outfit through terminal options.
- These visual details are added using the Decorator Pattern.

---

## Object-Oriented Principles

- **Encapsulation**: Pet state (happy, hungry, sick) and stats (hunger, mood, energy) are encapsulated in classes, private and managed via getters/setters.
- **Abstraction**: Interfaces like `PetState` and `PetObserver` which defines a generalized interface for pet moods and observer behavior.
- **Polymorphism**: Actions and states implemented with interchangeable behavior (Ex. Different pet states override the respond() method differently)
- **Inheritance**:  a1Entity inherits from tamaEntity, obtaining position and image attributes
- **Modularity**: Clean class separation and package structure



---

## Design Patterns Used

| Pattern        | Purpose                                                    |
|----------------|------------------------------------------------------------|
| **State**       | Changes pet behavior based on its emotional/physical state |
| **Command**     | Runnable game loop, Key handling                           |
| **Observer**    | Notifies UI/loggers of changes to pet's status             |
|  **Decorator**  | Allows room and outfit customizations to be layered modularly|

**Additional Explanation:**
- **State Pattern**: States like `HappyState`, `HungryState`, and `SickState` implements the `PetState` interface and define their own responses, helping manage dynamic pet behavior.
- **Observer Pattern**: `VirtualPet` maintains a list of observers (e.g. `PetStatusLogger`) and notifies/logs them when stats and states change.
- **Command Pattern**: Each key press in the game corresponds to a pet action (`feed`, `play`, `rest`) that is conceptually wrapped as a command. These actions are abstracted into executable operations, and the invoker (game input handler) triggers them.
- **Decorator Pattern**: Enables the user to dynamically add decorations like plants, wallpaper, hats, and outfits to the pet and room without modifying base code.

---

## Potential Use Case

- A digital wellness/education app
- Used for habit building, emotional care, or gamified responsibility
- Similar to real apps used by kids, students, or even adults

---

## Result

The game window displays a 2x2 tile screen:
- The pet is drawn in the center.
- Image changes based on the pet's current state.
- When the user presses keys, the pet reacts and updates stats in real time.
  
**Display Examples:**
- Happy face when pet is well maintained.
- Drooling face when hungry.
- Sleeping face when resting.

---

## Future Add-ons

- [ ] Modular Pet Types
- [ ] Full GUI feedback
- [ ] Sprites and Animations
- [ ] Background Decorator Art
---

## Project Structure

```
/src
  ├── entity/
  │     ├── a1Entity.java
  │     ├── tamaEntity.java
  ├── state/
  │     ├── PetState.java
  │     ├── HappyState.java
  │     ├── HungryState.java
  │     └── SickState.java
  ├── decorator/
  │     ├── BasicRoom.java
  │     ├── DressDecorator.java
  │     ├── DressEnvironment.java
  │     ├── HatDecorator.java
  │     ├── PetEnvironment.java
  │     ├── PlainTama.java
  │     ├── PlantDecorator.java
  │     ├── RoomDecorator.java
  │     ├── TamaDecorator.java
  │     └── WallpaperDecorator.java
  ├── observer/
  │     ├── PetObserver.java
  │     ├── UIUpdater.java
  │     └── PetStatusLogger.java
  ├── model/
  │     └── VirtualPet.java
  ├── model/
  │     └── VirtualPet.java
  ├── main/
  │     ├── GamePanel.java
  │     ├── KeyHandler.java
  │     ├── Setup.java
  │     └── Main.java
  └── /res
        ├── tama/
        │     ├── tama_drooling.png
        │     ├── tama_eating.png
        │     ├── tama_happy.png
        │     ├── tama_neutral.png
        │     ├── tama_sad.png
        │     ├── tama_sick.png
        │     ├── tama_sleeping.png
        │     └── tama_urushi.png
        └── hand.png
```

---

## UML Diagrams

```

+------------+
|   Main     |    🟢 Step 1
+------------+
| + main()   |  // Entry point of program. Creates room decoration, tama decoration and simulator GamePanel. 
+------------+


+------------------------------------------------+    
|                 GamePanel                      |    🟢 Step 3
+------------------------------------------------+
| - originalTileSize: int                        |
| - scale: int                                   |
| - tileSize: int                                |
| - maxScreenCol, maxScreenRow: int              |
| - screenWidth, screenHeight: int               |
| - FPS: int                                     |
| - keyH: KeyHandler                             |
| - gameThread: Thread                           |
| - tama: a1Entity                               |
| + GamePanel()                                  |
| + startGameThread(): void                      |
| + run(): void                                  |  // main game loop
| + update(): void                               |
| + paintComponent(g: Graphics): void            |  // draw graphics
+------------------------------------------------+
      |
      v
+-----------------------------------------------+        +-------------------------+
|                   a1Entity                     |------->|      tamaEntity         |   🟢 Step 4
+-----------------------------------------------+        +-------------------------+
| - gp: GamePanel                                |        | - x, y: int             |
| - keyH: KeyHandler                             |        | - state: String         |
| - vPet: VirtualPet                             |        | - BufferedImages        |
| - lastStatDecayFrame: int                      |        +-------------------------+
| - statDecayInterval: int                       |
| + a1Entity(gp, keyH): constructor              |
| + setDefaultValues(): void                     |
| + getPlayerImage(): void                       |
| + update(): void                               |  // check key press, update state
| + draw(g2: Graphics2D): void                   |  // draw pet image
+-----------------------------------------------+
      |                                 |
      v                                 v
+------------------------+   +----------------------------+
|     VirtualPet         |   |        KeyHandler          |   🟢 Step 5
+------------------------+   +----------------------------+
| - hunger: int          |   | - aPressed, fPressed, etc. |
| - energy: int          |   | + keyPressed(), Released() |
| - mood: int            |   | + keyTyped()               |
| - state: PetState      |   +----------------------------+
| - observers: List<>    |
| + VirtualPet(...)      |
| + get/set Hunger/Energy/Mood |
| + feed(), play(), rest()|
| + updateStats(): void  |
| + attach(), detach()   |
| + notifyObservers()    |
| + checkState(): void   |
+------------------------+
                  
                          
+--------------+  +------------------------+  
|  PetState    |  |    PetObserver         |  🟢 Step 6-8
+--------------+  +------------------------+  
| + respond(p) |  | + update(pet): void    |  
+--------------+  +------------------------+ 
     ^                    ^        ^           
     |                    |        |        
+-------------------+ +--------------------+ 
| HappyState        | | PetStatusLogger    | 
| - moodDrop: int   | | + update(pet): void| 
| + respond(pet)    | |                    | 
+-------------------+ +--------------------+ 
| HungryState       | | UIUpdater          |
| + update(pet)     | | + update(pet): void| 
| - moodDrop: int   | +--------------------+                       
| - energyDrop: int |
| + respond(pet)    |
+-------------------+
| SickState         |
| - moodDrop: int   |
| - energyDrop: int |
| + respond(pet)    |
+-------------------+

+-----------------------+                        +-------------------------+
|   PetEnvironment      |  <<interface>>         |   DressEnvironment      |  <<interface>>
+-----------------------+                        +-------------------------+          
| + describe(): String  |                        | + describeDress(): String |
+-----------------------+                        +-------------------------+
       ^                                                  ^
       |                                                  |
+-----------------------+                        +-------------------------+
|     BasicRoom         |                        |      PlainTama          |          🟢 Step 2
+-----------------------+                        +-------------------------+
| + describe(): String  |                        | + describeDress(): String |
+-----------------------+                        +-------------------------+
       ^                                                  ^
       | extends                                          | extends
+---------------------------+                  +-----------------------------+
|     RoomDecorator         |                  |       TamaDecorator         |
+---------------------------+                  +-----------------------------+
| - baseRoom: PetEnvironment|                  | - baseDress: DressEnvironment |
| + describe(): String      |                  | + describeDress(): String     |
+---------------------------+                  +-----------------------------+
       ^                                                  ^
       |                                                  |
+------------------------------+            +-------------------------------+
|     PlantDecorator           |            |        DressDecorator         |
+------------------------------+            +-------------------------------+
| - treeType: String           |            | - dressType: String            |
| + describe(): String         |            | + describeDress(): String      |
+------------------------------+            +-------------------------------+

+------------------------------+            +-------------------------------+
|     WallpaperDecorator       |            |         HatDecorator          |
+------------------------------+            +-------------------------------+
| - wallpaperType: String      |            | - hatType: String              |
| + describe(): String         |            | + describeDress(): String      |
+------------------------------+            +-------------------------------+
```

---

## Controls for Keyboard

- `R` : Rest Action
- `F` : Feed Action
- `P` : Play Action
- `A` : [Debug for showing happy state]

---

## Pet Stats

- `hunger` : [0-100] 0 = full, 100 = starving
- `energy` : [0-100] 0 = exhausted, 100 = fully rested
- `mood` : [0-100] 0 = sad, 100 = happy (Sad state not implemented)

---

## ETC

- set `res` to be a root source file for working images