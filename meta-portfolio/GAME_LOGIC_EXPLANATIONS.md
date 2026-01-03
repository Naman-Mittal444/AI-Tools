# Game Logic Explanations & Tool Concepts

This document contains detailed explanations of core game mechanics and tool concepts for implementation reference.

---

## 🎮 Game Logic Explanations

### 1. 2048 (Sliding Tile Puzzle)

#### Concept: Grid Logic
**Explanation:** The game board is a $4 \times 4$ array (list of lists) initialized with zeroes (empty cells). Tiles are stored as the integer value (2, 4, 8, etc.).

**Programming Focus:** 2D Array Management and checking for the "2048" win condition.

**Implementation Notes:**
- Initialize a 4x4 grid with zeros
- Track tile values as integers (2, 4, 8, 16, 32, ..., 2048)
- Check for win condition when any cell reaches 2048
- Check for loss condition when grid is full and no moves are possible

#### Concept: Directional Movement
**Explanation:** When a key is pressed (Up, Down, Left, Right), all tiles must slide as far as possible in that direction until they hit the edge or another tile.

**Programming Focus:** Iterative Loop that shifts all non-zero numbers to the edge, leaving zeroes behind.

**Implementation Notes:**
- For each direction, iterate through rows/columns
- Move all non-zero tiles toward the edge
- Fill empty spaces with zeros
- Process tiles in the correct order (e.g., for right: process from right to left)

#### Concept: Merge Handling
**Explanation:** If two identical, adjacent tiles collide during the slide, they merge into one tile whose value is their sum (e.g., $4 + 4 \to 8$). Only one merge per pair per turn.

**Programming Focus:** The core logic must handle the merge before the shift, ensuring the merged tile cannot merge again in the same move.

**Implementation Notes:**
- After shifting, check for adjacent identical values
- Merge pairs from the direction of movement (e.g., for right: merge rightmost pairs first)
- Mark merged tiles to prevent double-merging in the same turn
- Only merge once per tile per move

#### Concept: New Tile Spawn
**Explanation:** After every valid move, a new tile (usually a 2, sometimes a 4) must appear in a random empty cell on the grid.

**Programming Focus:** Random number/position generation and checking for empty spots (== 0).

**Implementation Notes:**
- After a valid move, find all empty cells (value == 0)
- Randomly select one empty cell
- Spawn tile with value 2 (90% chance) or 4 (10% chance)
- Only spawn if there are empty cells available

---

### 2. Hangman (CLI/Web)

#### Concept: Word Selection
**Explanation:** The program must choose a secret word from a list (or file) of words.

**Programming Focus:** File I/O (to read the word list) and using a random function to select the word.

**Implementation Notes:**
- Maintain a word list (array or file)
- Use random selection to pick a word
- Store the selected word as the secret word
- Consider difficulty levels (easy, medium, hard) based on word length

#### Concept: Display State
**Explanation:** The player only sees placeholders (e.g., underscores _ _ _ _) for the secret word. This display updates as letters are guessed correctly.

**Programming Focus:** String Manipulation (replacing _ with the correct letter) and maintaining a guessed_letters list.

**Implementation Notes:**
- Initialize display string with underscores matching word length
- Track guessed letters (both correct and incorrect)
- Update display when correct letter is guessed
- Show correctly guessed letters in their positions
- Keep incorrect guesses visible (e.g., "Wrong: A, B, C")

#### Concept: Guessing and Tries
**Explanation:** The player inputs a single letter. The program checks if the letter is in the secret word. If incorrect, the tries counter decreases (and the "hangman" ASCII art advances).

**Programming Focus:** Conditional logic (if/else) to check for a match and counter variable management.

**Implementation Notes:**
- Accept single letter input (validate input)
- Check if letter exists in secret word
- If correct: reveal all instances of the letter
- If incorrect: decrement tries counter, add to wrong guesses
- Update hangman drawing based on remaining tries
- Prevent duplicate guesses

#### Concept: Win/Loss Condition
**Explanation:** Win: The displayed word no longer contains any placeholders. Loss: The tries counter reaches zero.

**Programming Focus:** A while loop controls the game until the word_completion is achieved or tries == 0.

**Implementation Notes:**
- Check win condition: no underscores remaining in display
- Check loss condition: tries counter reaches 0
- Game loop continues until win or loss
- Display appropriate win/loss message
- Option to play again

---

### 3. Tetris (Falling Block Game)

#### Concept: The Grid/Board
**Explanation:** A tall 2D array (usually $10$ columns by $20$ rows) represents the "well" where blocks fall. Cells are marked as empty or solid (fixed) blocks.

**Programming Focus:** 2D Array Representation and Coordinate System to track position.

**Implementation Notes:**
- Create 10x20 (or 10x24) grid
- Use 0 or false for empty cells
- Use 1 or color codes for filled cells
- Track fixed blocks separately from falling piece
- Coordinate system: (row, col) or (x, y)

#### Concept: The Tetromino
**Explanation:** Each falling piece (I, J, L, O, S, T, Z) is a separate data structure, a 4-block shape that has its own position and rotation.

**Programming Focus:** Object-Oriented Programming (OOP) or a structure to define the 7 shapes and their possible rotations (e.g., $90^\circ$ rotation matrices).

**Implementation Notes:**
- Define 7 tetromino shapes (I, J, L, O, S, T, Z)
- Store shapes as 4x4 or 3x3 matrices
- Implement rotation states (0°, 90°, 180°, 270°)
- Track current piece position (x, y)
- Track current rotation state
- Pre-calculate or compute rotated shapes

#### Concept: Movement & Collision
**Explanation:** The piece constantly moves down. Player input moves it Left/Right and rotates it. Before any movement, the program must check if the new position overlaps with solid blocks or the walls/floor.

**Programming Focus:** Collision Detection (checking the four blocks of the Tetromino against the static board array).

**Implementation Notes:**
- Automatic downward movement (timer-based)
- Handle left/right movement input
- Handle rotation input
- Before any move, check collision:
  - Check boundaries (walls)
  - Check floor
  - Check against fixed blocks
- Only execute move if no collision detected
- Implement "ghost piece" preview (optional)

#### Concept: Locking & Line Clear
**Explanation:** Once a piece collides with a solid block or the floor, it locks into place, and its shape is permanently transferred to the main board array. The program then checks if any full horizontal lines are formed. If so, those lines are cleared, and all blocks above them shift down.

**Programming Focus:** Array Row Checking and Shifting/Splicing the 2D array data to move blocks down.

**Implementation Notes:**
- When piece can't move down, lock it
- Transfer piece blocks to main board
- Check each row for completeness (all cells filled)
- Remove complete rows
- Shift all rows above down
- Award points based on lines cleared
- Check for game over (blocks reach top)

---

## 🌍 Creative World Time Tool Concept

### Tool Name: Global Clock Dashboard

A data visualization tool focusing on time zones that displays current times across the world in an interactive, visually appealing format.

### Technical Requirements

#### Feature: Data Source
**Implementation Detail:** A list of key cities/countries and their official IANA Time Zone IDs (e.g., Europe/London, Asia/Kolkata).

**Technical Focus:** External API/Library: Use a robust library like Python's `pytz` or JavaScript's `Intl.DateTimeFormat` to handle time zone conversions, Daylight Saving Time (DST), and offsets.

**Implementation Notes:**
- Use IANA timezone database for accurate timezone data
- Python: `pytz` or `zoneinfo` (Python 3.9+)
- JavaScript: `Intl.DateTimeFormat` with timeZone option
- Handle DST transitions automatically
- Support for major cities worldwide
- Update times in real-time (every second or minute)

#### Feature: Creative Display
**Implementation Detail:** For each location, display the flag, full country name, current time in 12-hour clock format (analog/digital), and a time difference relative to the user's current time (e.g., +5:30).

**Technical Focus:** Frontend Framework (React/Vue/JS): Use SVG or CSS to render flags and use dynamic styling (e.g., a dark border/theme for night-time zones, light for day).

**Implementation Notes:**
- Display format:
  - Country flag (emoji or SVG)
  - City/Country name
  - Current time (12-hour format with AM/PM)
  - Time difference from user's timezone
  - Optional: 24-hour format toggle
- Visual indicators:
  - Day/night theme based on local time
  - Color coding for time zones
  - Analog clock visualization (optional)
- Responsive grid layout
- Smooth animations for time updates

#### Feature: Interactive Map (Optional)
**Implementation Detail:** A shaded map highlighting the world's time zones. Hovering over a region displays the current time.

**Technical Focus:** D3.js or Leaflet.js with GeoJSON data to visually represent time zones.

**Implementation Notes:**
- Use D3.js for SVG-based timezone visualization
- Or Leaflet.js for interactive map
- GeoJSON data for timezone boundaries
- Hover tooltips showing time information
- Click to add location to dashboard
- Color-code regions by timezone offset
- Optional: Show day/night terminator line

#### Feature: Customization
**Implementation Detail:** Allow the user to "pin" their most-used time zones to the top of the dashboard.

**Technical Focus:** Local Storage to save user preferences across sessions.

**Implementation Notes:**
- Save pinned locations to localStorage
- Drag-and-drop to reorder locations
- Add/remove locations from dashboard
- Save display preferences (12/24 hour, theme)
- Export/import configuration
- Remember last viewed locations

### Additional Implementation Ideas

1. **Alarm/Reminder System:** Set alarms for specific times in different timezones
2. **Meeting Planner:** Find best meeting times across multiple timezones
3. **World Clock Widget:** Embeddable widget for websites
4. **Time Converter:** Convert between timezones with date support
5. **Daylight Saving Time Indicators:** Visual indicators for DST transitions
6. **Historical Time Data:** Show time differences at different historical periods

---

## 📝 Implementation Checklist

### 2048 Game
- [ ] Create 4x4 grid data structure
- [ ] Implement directional movement (Up, Down, Left, Right)
- [ ] Implement merge logic (prevent double-merge)
- [ ] Implement new tile spawning
- [ ] Add win condition (2048 tile)
- [ ] Add loss condition (no valid moves)
- [ ] Add score tracking
- [ ] Add animations/transitions

### Hangman Game
- [ ] Create word list (file or array)
- [ ] Implement word selection (random)
- [ ] Create display state (underscores)
- [ ] Implement letter guessing logic
- [ ] Track correct/incorrect guesses
- [ ] Implement tries counter
- [ ] Add ASCII art for hangman
- [ ] Add win/loss conditions
- [ ] Add difficulty levels (optional)

### Tetris Game
- [ ] Create game board (10x20 grid)
- [ ] Define 7 tetromino shapes
- [ ] Implement rotation system
- [ ] Implement movement (left, right, down)
- [ ] Implement collision detection
- [ ] Implement piece locking
- [ ] Implement line clearing
- [ ] Implement row shifting
- [ ] Add score system
- [ ] Add level progression
- [ ] Add game over detection

### Global Clock Dashboard
- [ ] Set up timezone data source
- [ ] Implement time conversion logic
- [ ] Create location display components
- [ ] Add flag/country name display
- [ ] Implement time difference calculation
- [ ] Add day/night theme switching
- [ ] Implement localStorage for preferences
- [ ] Add location pinning feature
- [ ] Add interactive map (optional)
- [ ] Add real-time updates

---

*This document serves as a reference guide for implementing these games and tools. All concepts are explained with their core programming requirements and implementation notes.*

